# TYPESCRIPT

## TIPOS DE DATOS

### String

```typescript
const str1: string = "cadena comillas dobles";
const str2: string = 'cadena comillas simples';

// Template literals
const nombre: string = "Juan";
const edad: number = 34;

const str3: string = `Hola, mi nombre es ${nombre} y tengo ${edad} años`;
```

### Number

```typescript
// enteros
const num1: number = 10;
// flotante
const num2: number = 10.4;
// exponentes
const num3: number = 2.5e3;		// 2.5 * 10^3 = 2500
// exponente negativo
const num4: number = 1.5-e2;	// 1.5 * 10^-2 = 0.015
// hexadecimales prefijo 0x
const num5: number = 0xA; 		// Valor decimal: 10
// octales prefijo 0o
const num6: number = 0o11;		// Valor decimal: 9
// binarios prefijo 0b
const num7: number = 0b1010;	// Valor decimal: 10
```

### Boolean

```typescript
const bool1: boolean = true;
const bool2: boolean = false;
```

### Undefined y null

```typescript
// declaracion undefined
let variableUndefined: undefined;
// asignacion undefined
variableUndefined = undefined;

// declaracion null
let variableNull: null;
// asignacion null
variableNull = null;
```

### Objeto

```typescript
const programador = {
  nombre: "fernando",
  edad: 42,
  casado: false,
 	lenguajes: ["Java", "Javascript", "Python", "Go"],
}
```

### Arrays

```typescript 
const numeros: number[] = [1, 2, 3, 4, 5];

const nombres: string[] = ["Fernando", "Pepe", "Timmy"];

const valoresBool: boolean[] = [true, false, true];
```

### Enum

```typescript
enum Semana {
  Lunes,
  Martes,
  Miércoles,
  Jueves,
  Viernes,
  Sábado,
  Domingo,
}

enum Colores {
  Rojo = "rojo",
  Verde = "verde",
  Azul = "azul",
}
```

### Funciones

```typescript
// funcion con tipado explicito
function sumar(a: number, b: number): number {
  return a + b;
}

// funcion de flecha con retorno implícito
const dividir = (a: number, b: number) => a / b;

// funciones con parametros opcionales:
function saludar(nombre: string, edad?: number): string {
  if(edad !== undefined) {
    return `Hola, mi nombre es ${nombre} y tengo ${edad} años.`;
  } else {
    return `Hola, mi nombre es ${nombre}.`;
  }
}

// Funciones con parametros por defecto
function saludar2(nombre: string, edad: number = 30): string {
  return `Hola, mi nombre es ${nombre} y tengo ${edad} años.
}
```

### Clases

```typescript
class Persona {
  nombre: string;
  
  constructor(nombre: string) {
    this.nombre = nombre;
  }
  
	saludar() {
    console.log(`Hola, mi nombre es ${this.nombre}.`);
  } 
}
```

### Interfaces

```typescript
// Interface basica
interface Persona {
  nombre: string;
  edad: number;
}

// Interface con propiedades opcionales
interface Producto {
  nombre: string;
  precio: number;
  descripcion?: string;
}

// interface para funciones
interface Comparador {
  (a: number, b: number): boolean;
}

// interface para clases
interface Persona {
  nombre: string;
  edad: number;
  saludar(): void;
}

// implementar interface
class Aventurero implements Persona {
  nombre: string;
  edad: number;
  profesion: string;
  saludar(): void {
		console.log(`Hola, mi nombre es ${this.nombre}`)
  }
  
  constructor(nombre: string, edad: string) {
    this.nombre = nombre;
    this.edad = edad;
  }
}
```

### Types

```typescript
// Type basico
type Numero = number;

// Type basico objeto literal
type Personal = {
  nombre: string;
  edad: number;
}

// Type con union types
type Nombre = string | null;

// Type con propiedades opcionales
type Producto1 = {
  nombre: string;
  precio: number;
  descripcion?: string;
}

// Type para funciones
type Comparador1 = {
  (a: number, b: number): boolean
}

// Type para clases
type Persona2 = {
  nombre: string;
  edad: number;
  saludar(): void;
}
```

