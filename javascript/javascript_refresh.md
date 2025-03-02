# JAVASCRIPT

## Conceptos Básicos

### Variables

- las variables se pueden declarar con var y let
- var tiene ambito de funcion mientras que let tiene ambito de bloque definido por brackets
- **siempre utilizar let**

```javascript
function myFunc() {
	var foo = "Foo";
	{
		foo = "Boo"
		let bar = "Bar";
		typeof(bar);  // string
	}
	console.log(foo); // Boo
	console.log(bar); // ReferenceError
}
```
### Constantes

- Se definen  con la *keyword* const
- Tienen ámbito de bloque, como las variables definidas por **let**
- Se puede definir una constante como resultado de una expresión.

### Interpolación variables en string
- Se hace utilizando las comillas invertidas ``
```javascript
let nombre = "Anduil";
console.log(`Hola, ${nombre}!`);

let a = 1;
let b = 2;
console.log(`La suma de a + b es ${a + b}`);
```


### Igualdad

- Doble igual (==) compara solo el valor.
- Triple igual (===) compara tanto el valor como el tipo.
- Es preferible usar el ===.

```javascript
3 == "3"	// true
3 === "3"	// false
3 === 3		// true
1 !== 1   // false
```

### Nullish coalescing

- Se escribe ??
- Devuelve el operador de la izquierda si existe, o el de la derecha si  el de la izquierda es null o undefined.

```javascript
let a = 5;

let c = a ?? 12;			// 5
let d = b ?? 8;				// 8
```

### Ternary operator

```javascript
let a = 10;
let greaterThan5 = a > 5 ? true : false;   // true;
```



### Operadores de comparación

Los operadores de comparación tienen la particularidad de devolver el último valor comparado.

```javascript

true && 1 = 1  // 1 es el ultimo valor comparado asi que lo devuelve
0 && true = 0  // como 0 es igual a false no comprueba más, y lo devuelve.
1 && "hello"   // devuelve "hello"

const port = process.env.PORT || 3000	// si PORT existe lo devuelve, y sino 														//	devuelve 3000

```

### Condicionales
if else if
```javascript
if(i===0) {
	console.log("i es 0");
} else if(i>0 && i<5) {
	console.log("i vale entre 1 y 5");
} else {
	console.log("i es mayor que 5");
}



```
switch
```javascript
switch(valor) {
	case 0: console.log("valor 0");break;
	case 1: console.log("valor 1");break;
	default: console.log("valor ni 0 ni 1");
}
```

### Funciones

*function* nombreFunc(param1, param2, ...) {}
```javascript
function saludar(nombre) {
	return `Hola ${nombre}!`
}
```

### Funciones como clases 

(deprecado, ahora se usa la keyword class)

```javascript
// por convención, la primera letra del nombre de la funcion de clase en mayúscula
function Inventario(nombre){
    this.nombre = nombre;
    this.articulos = [];
}   	
// para añadir metodos usamos prototype
Inventario.prototype.getNombre = function() {
    return this.nombre;
}
Inventario.prototype.add = function(articulo, cantidad) {
    this.articulos[articulo] = cantidad;
}
Inventario.prototype.getCantidad = function(articulo) {
    return this.articulos[articulo];
}
// Inicializar un objeto de esta clase
let libros = new Inventario("libros");
libros.getNombre();		// libros
libros.add("Aprendiendo JavaScript", 5);
libros.getCantidad("Aprendiendo JavaScript");
console.log(libros.getCantidad("Aprendiendo JavaScript"))

```

### Clases

- Siempre tienen un constructor que se declara con la palabra clave constructor. Si no se declara se genera un constructor vacío automaticamente.

```javascript
class Car {
  constructor(name, year) {
    this.name = name;
    this.year = year;
  }
  age() {
    const date = new Date();
    return date.getFullYear() - this.year;
  }
}

const myCar = new Car("Ford", 2014);
console.log(`My car is ${myCar.age()} years old.`);
```



## BUCLES

### While

```javascript
while(condition) {
  // code
}
```

### Do while

```javascript
do {
  // el codigo siempre se ejecuta al menos una vez, ya que la condicion se chequea tras la ejecución del codigo
} while (condition)
```

### For

```javascript
for(let i=0; i<num; i++) {
  // code
}
```

### Arrays

```javascript
// declaración array con valores ( o vacío)
var myArray = [];
// un array es un conjunto de objetos que no tienen porque ser del mismo tipo
var myArray = ["a", 3, false];
// declaración array con tamaño mediante new
var myArray = new Array(100);
// el array aumenta de tamaño cuando es necesario automaticamente 
```

### Array.forEach

```javascript
var myArray = ["a", true, "2", 4];
// con funcion anonima
myArray.forEach(function(element, index) {
    console.log(`element at ${index} is ${element}`);
});
// con funcion flecha
myArray.forEach((element, index) => {
	console.log(`element at ${index} is ${element}`);
});
```

## TIPOS DE DATOS

### Object

Un objeto es un conjunto estructurado de variables (propiedades) y funciones (métodos).

```javascript
const libro = {
  titulo: "Aprendiendooslo JavaScript",
  autor: "Carlos Azaustre",
  numPaginas: 100,
  editorial: "carlosazaustre.es",
  precio: "24.90",
  leer: ()=> {
    console.log(`${titulo}`);
  },
};

// formas de acceso a propiedades del objeto
libro.titulo
libro ["titulo"]
// setear (que el objeto sea const no impide que sus propiedades cambien)
libro.titulo = "Como asar cabras";
libro["titulo"] = "Corchetes";

// instanciacion alternativa con new
let libro = new Object({
  // titulo: ...,
  // autor: ...,
});

// Objetos anidados
let libro =  {
  titulo: "",
  autor: {
    nombre:"",
    edad:0,
    redes: {
      twitch:"",
      tiktok:"",
    }
  }
}

libro.autor.nombre
libro["autor"]["redes"]["twitch"]

const coche1 = {marca:"Tesla", modelo:"X"}
const coche2 = {marca:"Tesla", modelo:"X"}

coche1 === coche2 // false, porque no tienen la misma referencia de memoria
coche1.marca === coche2.marca // true

coche3 = coche1
coche3 === coche1 // true, tienen la misma referencia
```

### Number

Nos permite representar números en coma flotante de doble precisión (64 bits)

```javascript
25 			// numero entero
25.5 		// decimal
0x1F 		// Numero hexadecimal (31)
0xFF 		// Numero hexadecimal (255)
5.4e2 	// Potencia, 5.4 * 10^2 540
1/0			// infinity
-1/0		// -infinity
1e1000 	// infinity, porque el numero es demasiado grande para representarlo
"a"/15	// NaN - Not a Number


let numero = 5;
let numero = new Number(5); // esta forma nunca se utiliza

parseInt("15") 				// parsea la string a decimal, 15
parseInt("1111", 2) 	// parsea la string a binario, 15
parseInt("1111", 16)	// parsea la string a hexadecimal, 4369

parseFloat("5.23")

let n = 2.5678
n.toFixed(2); 		// devuelve una string con dos decimales redondeando "2.57"
n.toFixed(0);			// devuelve 0 decimales redondeando "3"

n.toExpotential(2); // 2.57e+0;

n.toString();			// "1111"
```

### Array

Colección de datos de cualquier tipo.

```javascript
let miArray = [1, "2", 3];	// un array puede contener varios tipos
let miArray = [
  {nombre: "pepe", edad: 24}, 
  {nombre: "titi", edad: 12}
];	// array de objetos

miArray[0].nombre;	// pepe
miArray[0].nombre.length // 4

miArray = [	// array de arrays
  [2, 4],
  [5, 6],
];

miArray[0][1];  // 4

miArray =  [3, 6, 1, 4];
miArray.sort();					// [1, 3, 4, 6]
miArray.pop();					// 6 - saca (y returns) del array el ultimo elemento
miArray;								// [1, 3, 4]
miArray.push(2);				// mete el valor en el array (y returns la posicion en la que se mete)
miArray;								// [1, 3, 4, 2]
miArray.sort();					// [1, 2, 3, 4]
miArray.reverse();			// [4, 3, 2, 1]

miArray.join("_");			// "4_3_2_1" une todos los elementos del array por el separador indicado. el resultado se devuelve como string

miArray = [2, 4, 6, 8];
let raices = miArray.map((item)=>Math.sqrt(item));			// map ejecuta una función para cada elemento del array

miArray = [1,2,3,4,5,6,7,8,9,10];
let multiplosDe3 = miArray.filter((item)=>item %3 === 0);  // [3, 6, 9]

miArray = [1,2,3,4,5,6,7,8,9,10];
miArray.slice(2)				// devuelve el array cortado a partir del elemento 2 [3, 4, 5...]

miArray.slice(2, 4)			// devuelve el array cortado de la posicion 2 incluida a la 4 excluida - [3, 4]

miArray.slice(2, -2)    // desde la posición 2 incluida a la posicion 2 empezando por el final sin incluir - [3, 4, 5, 6, 7, 8]
```

### String

Una string se comporta como un array de caracteres

```javascript
"javascript"[2];										// "v"
"javascript".length									// 10
"javascript".charCodeAt(2)  				// el numero en UNICODE del caracter en esa posición
"javascript".indexOf("scr")  				// 4, indica a partir de que index aparece el texto indicado
"javascript".indexOf("palal") 			// -1, si no encuentra el texto devuelve -1
"javascript".substring(2, 4)				// "va", from first index inclusive to last index exclusive

const fecha = new Date().toString()	// 'Sat Mar 01 2025 22:15:23 GMT+0100 (Central European Standard Time)'
fecha.split(" ")										// [ 'Sat', 'Mar', '01', '2025', '22:15:23', 'GMT+0100', '(Central', 'European', 'Standard', 'Time)' ] - returns string as array splitted in elements by the string provided as argument	

```

## ARRAYS Y MÉTODOS

```javascript
const posts = [
  {
    id: 1,
    title: "Mi primer post",
    tags: ["javascript", "webdevelopment", "angular"],
  }, {
    id: 2,
    title: "Mi segundo post",
    image: "https://img.com/2",
    tags: ["javascript", "webdevelopment", "react"],
  }, {
    id: 3,
    title: "Mi tercer post",
    image: "https://img.com/3",
    tags: ["javascript", "webdevelopment", "vue"],
  },
];

posts.find(post => post.title === "Mi segundo post");  // returns the whole post

posts.some(post => post.id === 1);									// true if exists, false if not
posts.some(post => post.tags.includes("react"));		// true

posts.every(post => post.tags.includes("angular")); // false, with every it has to be in all posts
posts.every(post => post.tags.includes("javascript")) // true

posts.map(post => post.title);			// returns an array with titles of posts

posts.filter(post => post.tags.includes("angular"))  // returns post 1
posts.filter(post => post.image !== undefined)  // returns posts 1 and 2

posts.map(post=>post.id).reduce((total, id)=> total + id);  // 6, reduce gets an array and returns an unique valor, in this case the sum of ids values of all the posts

```

## THIS

- Se refiere al contexto donde se está ejecutando un trozo de código.

```javascript
// ECMASCRIPT 5
let obj = {
  foo: function() { return "foo"},
  bar: function() { 
    document.addEventListener("click", function(event) {
    	this.foo;
  	}).bind(this); // si no bindeamos el this interno al this externo, entonces el this.foo se referiria al contexto del document, y no de la variable obj.
  }
}

// ECMASCRIPT 6 - USAR ESTA SYNTAXIS
var obj = {
  foo: function() { return "foo"},
  bar: function() {
    document.addEventListener("click", event => this.foo()); // the arrow function makes the bind automatically
  }
}
```

## CLOSURES

- Un closure es una función autoejecutable que encapsula una serie de variables y definiciones locales que solo serán accesibles si son devueltas con el operador return. Te permite tener estructuras de datos privadas.

```javascript
// Una funcion se puede almacenar como una variable
const saludar = function(nombre) {
  return `Hola ${nombre}`;
}

saludar();								// Hola undefined
saludar("Carlos");				// Hola Carlos

// una función puede tener otras funciones dentro de ellas, creando nuevos scopes
const a = "Hey!";
function global() {
  const b = "¿Qué ";
  function local() {
    const c = "tal?";
    return a + b + c;
  }
  return local;
}
// SIN CLOSURE
global();						// return [Function: local]
global()();					// return "Hey! ¿Qué tal?"

// CON CLOSURE
const closure = global();
closure();					// return "Hey! ¿Qué tal?"


// EJEMPLO COMPLETO DE CLOSURE
const miContador = (function() {
  let _contador = 0;			// el _ al principio es una convencion para indicar que queremos que la variable sea privada
  
  function incrementar() {
    return _contador++;
  }
 
  function decrementar() {
    return _contador--;
  }
  
  function valor() {
    return _contador;
  }
  
  return {
    incrementar,
    decrementar,
    valor,
  }
})();		// !!!!!! notese el () final de ejecución.

miContador.valor()					// 0
miContador.incrementar()		// 0
miContador.valor()					// 1
```

## ASINCRONÍA

### Promises

```javascript
const datos = [{
  id: 1,
  title: "Iron Man",
  year: 2008,
}, {
  id: 2,
  title: "Spiderman: Homecoming",
  year: 2017,
}, {
  id: 3,
  title: "Avengers: Endgame",
  year: 2019,
}];

let getDatos = () => {
  setTimeout(() => {
    return datos;
  }, 1500);									
}

console.log(getDatos());			// undefined, porque intenta retornar datos inmediatamente y como hay un timeout hasta que no pase el timeout no se van a poblar los datos

getDatos = () => {
  return new Promise((resolve, reject)=> {
    setTimeout(()=> {
      resolve(datos);
    }, 1500)
  })
};														// con una promesa esperamos hasta que se reciban los datos

getDatos()
  .then(data => console.log(data))	// con then recuperamos los datos de la promesa
	.catch(err => console.log(err));
```

### Async/Await

```javascript
async function fetchData () {
  try {
    const datosAwait = await getDatos();		// mas legible que las promesas, pero tiene que estar dentro de una funcion async
console.log(datos);
  } catch (e) {
    console.log(e);
  }
}

fetchData();

```

