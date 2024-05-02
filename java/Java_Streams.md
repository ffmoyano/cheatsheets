# Streams



## Introducción

Manejo de colecciones en java de manera funcional.

Por ejemplo, si queremos coger de una lista de personas a las 10 primeras menores de 18 años pasaremos de hacerlo con un for mejorado: 

```java
	List<Person> people = MockData.getPeople();
	int counter = 0;
    var youngOnes = new ArrayList<Person>();
    for (Person p : people) {
        if (p.getAge() <= 18) {
            youngOnes.add(p);
        }
        counter++;
        if (counter == 10) {
            break;
        }
    }

    for (Person p : youngOnes) {
        System.out.println(p);
    }
```

Pasamos a hacerlo con stream:

```java
MockData.getPeople().stream()
                .filter(p -> p.getAge() <= 18)
                .limit(10)
                .collect(Collectors.toList())
                .forEach(System.out::println);
```

Dando como resultado un bloque mucho mas fácil de leer y mucho más conciso.

En los pasos de un stream, si va a haber mas de una operación las tendremos que rodear todas de {}, y además separar cada una por ; además tendremos que especificar la palabra return si debemos devolver algo, lo cual no es necesario cuando es solo una operación. Por ejemplo, si en el *map* solo hay una operación:

```
List<Integer> ages = people.stream()
        .map(p -> p.getAge()).collect(Collectors.toList());
```

Pero si hay mas de una operación involucrada hay que meter curly braces y return.

```
List<Integer> agesPlus10 = people.stream()
        .map(p -> {
            p.getAge();
            p.setAge(p.getAge() +10);
            return p.getAge();
        }).collect(Collectors.toList());
```



Si queremos operar sobre **Arrays** tenemos que usar *Arrays.stream*:

```java
Arrays.stream(miArray).loquesea();
```



## IntStream

### Range

Para iterar por un rango de *ints* en lugar de usar: 

```java
for (int i = 0; i < 10; i++) {
    System.out.println(i);
}
```

Podemos usar la interfaz **IntStream** con los metodos *range* y *rangeClosed*, ambos aceptan dos *ints*, el inicial y el final, la diferencia es que el primer métodoexcluye el valor final y el segundo método lo incluye:

```java
IntStream.range(0,10).forEach(System.out::println);
IntStream.rangeClosed(0,9).forEach(System.out::println);
```

Podemos utilizar el **IntStream** para iterar por una lista, normalmente no es necesario, y es más fácil usar el método *stream*, pero puede resultar útil si necesitas conocer el *index* que estás recorriendo para hacer algo:

```java
List<Person> people = MockData.getPeople();
IntStream.range(0, people.size()-1)
        .forEach(index -> System.out.println(people.get(index)));
```

Si no necesitas el *index*, con el *stream* normal:

```java
people.forEach(System.out::println);
```

### Iterate

*Iterate* se diferencia de *range* en que, si bien acepta también dos parámetros y el primero es igualmente el valor de inicio, el segundo parámetro va a ser como va a cambiar el primer parámetro, vamos a poner un *limit* para que no opere indefinidamente:

```java
IntStream.iterate(0, operand -> operand + 1)
        .limit(20)
        .forEach(System.out::println);
```



## Min Max and Comparators

### Min

Podemos obtener el elemento menor de una colección utilizando el método *min*. Deberemos pasarle un comparador adecuado.

```
final List<Integer> numbers = ImmutableList.of(1, 2, 3, 100, 23, 93, 99);
int min = numbers.stream().min(Comparator.naturalOrder()).get();
```

### Max

Es similar al *min*, pero para obtener el elemento mayor de una colección.

```
final List<Integer> numbers = ImmutableList.of(1, 2, 3, 100, 23, 93, 99);
int max = numbers.stream().max(Comparator.naturalOrder()).get();
```



## Distinct and Collectors.toSet()

Podemos filtrar de una lista para que elimine todos los elementos repetidos con el método *distinct*

```java
List<Integer> differentNumbers = numbers.stream()
        .distinct().collect(Collectors.toList());
```

También podemos omitir la necesidad de usar distinct recolectando el stream en un **Set**, ya que éstos no permiten elementos repetidos.

```java
Set<Integer> differentNumbers = numbers.stream().collect(Collectors.toSet());
```



## Filter, Predicates And Transformations with Map

### Filter

Podemos filtrar una lista con el método *filter*, al cual le especificaremos la/s condicion/es que debe cumplir cada elemento de la lista para no ser eliminado por el filter. Por ejemplo en este caso recogeremos de una lista todos los coches con un precio inferior a 10.000, eliminando a todos los que superen ese límite:

```java
ImmutableList<Car> cars = MockData.getCars();
List<Car> cheapCars = cars.stream()
        .filter(c -> c.getPrice() < 10000)
        .collect(Collectors.toList());
```

### Predicate

Podemos declarar una operación de resultado *booleano* como *predicate* para luego insertarla en las operaciones de stream como *filter*. Por ejemplo, usando el predicado para la misma operación de antes:

```java
ImmutableList<Car> cars = MockData.getCars();
Predicate<Car> predicate = car -> car.getPrice() < 10000;
List<Car> cheapCars = cars.stream()
        .filter(predicate)
        .collect(Collectors.toList());
```

### Map

Podemos cambiar el tipo de objeto que nos devuelve el stream utilizando el método *map*. Por ejemplo, si tenemos una coleccion de personas y queremos recuperar solo una coleccion de integros con las edades de esas personas:

```java
List<Person> people = MockData.getPeople();
List<Integer> ages = people.stream()
                .map(p-> p.getAge()).collect(Collectors.toList());
```

También podemos devolver un objeto que no este directamente relacionado con el objeto original, por ejemplo:

```java
List<Person> people = MockData.getPeople();
List<PersonDTO> dtos = people.stream()
                .map(person ->
                        new PersonDTO(person.getId(), person.getFirstName(), person.getAge())
                ).collect(Collectors.toList());
```

Si en el objeto PersonDTO tenemos un método llamado *map* qhe hace lo mismo que un new PersonDTO(person.getId(), person.getFirstName(), person.getAge()) podemos usar el **Method Reference**:

```java
List<Person> people = MockData.getPeople();
List<PersonDTO> dtos = people.stream()
        .map(PersonDTO::map)
        .collect(Collectors.toList());
```

#### MapToDouble y Average

Podemos usar *mapToDouble* para pasar a una colección de dobles, también existen *mapToInt* o *mapToLong*, por ejemplo aquí cogemos una colección de coches, la pasamos a una colección de precios (double), y capturamos la media con *average*

```java
ImmutableList<Car> cars = MockData.getCars();
double averageCarPrice = (cars.stream().mapToDouble(Car::getPrice).average().orElse(0));
```



## Find Any y Find First

Estos métodos, aplicados por ejemplo a un *filter* devuelven el primer objeto encontrado:

findAny:

```java
Integer[] numbers = {1, 2, 3, 4, 1, 6, 7, 8, 9, 10};
Integer myInt = Arrays.stream(numbers).filter(n->n<10 && n > 5).findAny().orElse(0);
```

findFirst:

```java
Integer[] numbers = {1, 2, 3, 4, 1, 6, 7, 8, 9, 10};
Integer myInt = Arrays.stream(numbers).filter(n->n<10 && n > 5).findFirst().orElse(0);
```

La diferencia entre *findAny* y *findFirst* es que *findAny* no es determinístico y *findFirst* sí. Esto significa que findFirst siempre va a devolver el primer elemento del array, que cumple la condición, en este caso 6, pero findAny no siempre va a devolver el mismo, simplemente devolverá cualquier elemento del array que cumpla la condición y encuentre primero, no necesariamente el 6, el *tradeoff* de este método es que proporciona un mayor rendimiento en operaciones paralelas a costa de no obtener un resultado estable.



## Sacando Estadísticas del Stream

### Count

Count devuelve el número de ocurrencias de lo que le pasemos:

```java
long count = MockData.getPeople()
            .stream()
            .filter(p->p.getGender()
                    .equalsIgnoreCase("female"))
            .count();
```

### Min y Max sin comparator

Se puede utilizar el *min* y el *max* sin comparator siempre que se este mapeando a double, int o long, ponemos el getAsDouble (también podríamos poner *orElse*), pq sino devuelve un opcional.

```java
double min = MockData.getCars()
        .stream()
        .filter(c -> c.getColor().equalsIgnoreCase("yellow"))
        .mapToDouble(Car::getPrice)
        .min()
        .getAsDouble();
```

### Average

Como ya hemos visto antes, podemos utilizar *average* para calcular la media de una colección.

```java
double averagePrice = cars.stream()
        .mapToDouble(Car::getPrice)
        .average()
        .orElse(0);
```

### Sum

Nos permite sumar los valores de una colección. Si preveemos que el resultado va a ser muy grande podemos hacer un *BigDecimal* del resultado:

```java
double suma = cars.stream()
        .mapToDouble(Car::getPrice)
        .sum();
BigDecimal bd = new BigDecimal(suma);
```

### SummaryStatistics

Usando el metodo *sumaryStatistics* nos devolverá un objeto que contiene los valores del *average*, *count*, *max*, *min* y *sum*:

```java
List<Car> cars = MockData.getCars();
DoubleSummaryStatistics statistics = cars.stream()
    .mapToDouble(Car::getPrice)
    .summaryStatistics();
System.out.println(statistics);
    System.out.println(statistics.getAverage());
    System.out.println(statistics.getCount());
    System.out.println(statistics.getMax());
    System.out.println(statistics.getMin());
    System.out.println(statistics.getSum());
```



## Grouping

Podemos agrupar objetos de la colección por una propiedad que elijamos, esto nos devolvera un mapa con la propiedad elegida como clave, y como valor el tipo de colección original, en este caso vamos a agrupar una List<Car> por su propiedad make (marca).

```java
Map<String, List<Car>> grouping = MockData.getCars()
        .stream()
        .collect(Collectors.groupingBy(Car::getMake));

grouping.forEach((make, cars) -> {
  System.out.println(make);
  System.out.println("-----------");
  cars.forEach(System.out::println);
}) ;
```

### Grouping and counting

Podemos contar el número de elementos de cada grupo de la colección a la vez que los agrupamos de la siguiente manera:

```java
ArrayList<String> names = Lists
    .newArrayList(
        "John",
        "John",
        "Mariam",
        "Alex",
        "Mohammado",
        "Mohammado",
        "Vincent",
        "Alex",
        "Alex"
    );

Map<String, Long> counting = names.stream()
        .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));

counting.forEach((name, count) -> System.out.println(name + " > " + count));
```

Como son elementos simples se usa *Function.identity* para agruparlos, y se le pasa un **Collectors** *counting* adicional para que nos realice el conteo.

Si ejecutamos este código va a mostrar:

```
Alex > 3
Vincent > 1
Mariam > 1
Mohammado > 2
John > 2
```



## Reduce, Flatmap and Joining

### Reduce

Reduce nos permite pasar una colección de valores y obtener como resultado un único valor. Por ejemplo, vamos a sumar una lista de *ints* y obtener la suma como resultado:

```java
Integer[] integers = {1, 2, 3, 4, 99, 100, 121, 1302, 199};
int sum = Arrays.stream(integers).reduce(0, (a, b) -> a + b);
```

El 0 seria el valor que se le va a pasar originalmente a la variable *a*, la variable *b* seria el primer elemento del array. Entonces se haría la suma por primera vez, y el resultado pasaría a ser *a*, y el siguiente elemento del array la *b*, y así hasta iterar sobre todos los elementos.

También se puede expresar con el método *sum* de **Integer** así:

```java
int sum2 = Arrays.stream(integers).reduce(0, Integer::sum);
```

### FlatMap

Con flatmap podemos coger un grupo de colecciones y aplanarlas para dejar una sola colección:

```java
Lists.newArrayList(
      Lists.newArrayList("Mariam", "Alex", "Ismail"),
      Lists.newArrayList("John", "Alesha", "Andre"),
      Lists.newArrayList("Susy", "Ali")
  );
List<String> flattedList = arrayListOfNames.stream().flatMap(List::stream).collect(Collectors.toList());
System.out.println(flattedList);
```

### Joining

Con *Collectors.joining* podemos concatenar los elementos de una colección separados por el separador que elijamos:

```java
String namesJoined = names.stream().map(String::toUpperCase).collect(Collectors.joining(","));
```



## Collector detallado

Normalmente usamos *Collectors.toList*, o cualquier otro tipo de colección que queramos, pero también podemos especificar en el *collect* como queremos que interactúen las colecciones y sus elementos:

```java
List<String> emails = MockData.getPeople()
        .stream()
        .map(Person::getEmail)
        .collect(() -> new ArrayList<String>(),
                (list, element) -> list.add(element),
                (list1, list2) -> list1.addAll(list2)
        );
```

Esto también se puede expresar de una manera más compacta así:

```java
List<String> emails = MockData.getPeople()
        .stream()
        .map(Person::getEmail)
        .collect(ArrayList::new,
                ArrayList::add,
                ArrayList::addAll
        );
```