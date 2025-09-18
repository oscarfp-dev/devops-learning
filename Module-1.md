# Módulo 1 — Fundamentos de Programación con JavaScript y TypeScript

> Guía extensa y práctica para tu repositorio (README.md).  
> Incluye: conceptos + casos de uso + ejemplos + ejercicios y “best practices”.

---

## Índice

1. [Cómo usar esta guía](#cómo-usar-esta-guía)  
2. [JS vs TS en 2 líneas](#js-vs-ts-en-2-líneas)  
3. [Tipos (JS)](#tipos-js)  
   - [Primitivos](#primitivos) · [Objetos](#objetos) · [`typeof`](#typeof) · [Truthy/Falsy](#truthyfalsy)  
4. [Tipos (TS)](#tipos-ts)  
   - [Anotaciones e inferencia](#anotaciones-e-inferencia) · [Uniones, intersecciones, literales](#uniones-intersecciones-y-literales) · [Narrowing](#narrowing)  
5. [Operadores](#operadores)  
   - [Aritméticos y asignación](#aritméticos-y-asignación) · [Comparación (`==` vs `===`)](#comparación--vs-) · [Lógicos, `??`, `?.`](#lógicos--y-) · [Spread/Rest, destructuring](#spreadrest-y-destructuring) · [Otros (`in`, `instanceof`, `delete`)](#otros-in-instanceof-delete)  
6. [Condicionales](#condicionales)  
   - [`if/else`, `switch`, ternario, _guard clauses_]  
7. [Funciones](#funciones)  
   - [Declaración vs flecha](#declaración-vs-flecha) · [`this`, `arguments`, `new`](#this-arguments-new) · [Parámetros por defecto y rest](#parámetros-por-defecto-y-rest)  
8. [Objetos y colecciones](#objetos-y-colecciones)  
   - [Objeto literal, acceso y mutación](#objeto-literal-acceso-y-mutación) · [Arrays](#arrays) · [Map/Set/Weak*](#mapsetweak) · [Copias vs referencias](#copias-vs-referencias)  
9. [OOP en JS/TS](#oop-en-jsts)  
   - [Clases, herencia, `super`](#clases-herencia-super) · [Campos `public/private/protected/readonly` (TS)](#campos-publicprivateprotectedreadonly-ts) · [`implements` y contratos](#implements-y-contratos)  
10. [Asincronía](#asincronía)  
    - [Callbacks → Promises → `async/await`](#callbacks--promises--asyncawait) · [Paralelismo con `Promise.all`](#paralelismo-con-promiseall) · [Errores async](#errores-async)  
11. [TypeScript esencial (extra)](#typescript-esencial-extra)  
    - [Interfaces vs `type`](#interfaces-vs-type) · [Utility Types (`Record`, `Partial`, …)](#utility-types-record-partial-) · [Genéricos](#genéricos) · [`any/unknown/never`](#anyunknownnever) · [Enums vs literales](#enums-vs-literales)  
12. [Ejercicios por sección](#ejercicios-por-sección)  
13. [Cheat-Sheet](#cheat-sheet)  
14. [Setup rápido (Node/TS)](#setup-rápido-nodets)

---

## Cómo usar esta guía

- Lee cada sección, **ejecuta los ejemplos** y resuelve el ejercicio corto.  
- Copia/pega snippets en un Playground (Node/TS) y **experimenta** cambiando valores.  
- Prioriza entender **por qué** funciona, no solo “que funciona”.

---

## JS vs TS en 2 líneas

- **JavaScript (JS)**: tipado dinámico, corre en navegadores y Node.js.  
- **TypeScript (TS)**: superset de JS con **tipado estático** y herramientas que detectan errores **antes** de ejecutar.

---

## Tipos (JS)

### Primitivos
```js
let s = "texto";      // string
let n = 123;          // number (int y float)
let b = true;         // boolean
let u;                // undefined
let z = null;         // null
let big = 10n;        // bigint
let sym = Symbol("id"); // symbol
```

### Objetos
```js
const obj = { a: 1 };         // Object
const arr = [1, 2, 3];        // Array
function fn() {}              // Function
const fecha = new Date();     // Date
const regex = /abc/;          // RegExp
```

### `typeof`
```js
typeof 123      // "number"
typeof "hola"   // "string"
typeof true     // "boolean"
typeof null     // "object" (quirk)
typeof []       // "object"
typeof (()=>{}) // "function"
```

### Truthy/Falsy
Valores *falsy*: `0`, `""`, `null`, `undefined`, `NaN`, `false`.  
Todo lo demás es *truthy*.

```js
if ("") console.log("no");   // no entra
if ("hola") console.log("sí"); // sí entra
```

---

## Tipos (TS)

### Anotaciones e inferencia
```ts
let nombre: string = "Ana";
let edad: number = 30;
let activo: boolean = true;

// inferencia
let ciudad = "CDMX"; // TS infiere string
```

### Uniones, intersecciones y literales
```ts
type Estado = "pendiente" | "hecha";
let e: Estado = "pendiente";

type Base = { id: number };
type Avanzado = { permisos: string[] };
type Usuario = Base & Avanzado;
```

### Narrowing
```ts
function imprimirId(id: string | number) {
  if (typeof id === "string") {
    console.log(id.toUpperCase());
  } else {
    console.log(id.toFixed(2));
  }
}
```

---

## Operadores

### Aritméticos y asignación
```js
let x = 10;
x += 5; // 15
x **= 2; // 225
```

### Comparación (`==` vs `===`)
```js
0 == false    // true  (coerción)
0 === false   // false (estricto)
"5" == 5      // true
"5" === 5     // false
```

### Lógicos, `??`, `?.`
```js
const nombre = "" || "Anónimo";   // "Anónimo"
const activo = true && "OK";      // "OK"
const valor = undefined ?? "def"; // "def"

const obj = { user: { id: 1 } };
console.log(obj?.user?.id); // 1
```

### Spread/Rest y destructuring
```js
const arr = [1,2,3];
const copia = [...arr, 4];

function suma(...nums) {
  return nums.reduce((a,b)=>a+b,0);
}

const persona = { nombre:"Ana", edad:30 };
const { nombre } = persona;
```

### Otros (`in`, `instanceof`, `delete`)
```js
"nombre" in persona;   // true
arr instanceof Array;  // true
delete persona.edad;   // elimina propiedad
```

---

## Condicionales

```js
if (edad >= 18) {
  console.log("Mayor de edad");
} else {
  console.log("Menor de edad");
}

switch (nota) {
  case "A": console.log("Excelente"); break;
  case "B": console.log("Bien"); break;
  default: console.log("Necesita mejorar");
}

const estado = activo ? "ON" : "OFF";
```

Patrón útil: **guard clauses**
```js
function login(user) {
  if (!user) return "Usuario no válido";
  if (!user.activo) return "Desactivado";
  return "Bienvenido";
}
```

---

## Funciones

### Declaración vs flecha
```js
function normal(a,b){ return a+b; }
const flecha = (a,b) => a+b;
```

### `this`, `arguments`, `new`
```js
const obj = {
  valor: 42,
  normal: function(){ setTimeout(function(){ console.log(this.valor) },0); },
  flecha: function(){ setTimeout(()=>console.log(this.valor),0); }
};
obj.normal(); // undefined
obj.flecha(); // 42
```

### Parámetros por defecto y rest
```js
function saludar(nombre="Anónimo") {
  console.log("Hola " + nombre);
}

function sumar(...nums) {
  return nums.reduce((a,b)=>a+b,0);
}
```

---

## Objetos y colecciones

### Objeto literal, acceso y mutación
```js
const persona = {
  nombre: "Ana",
  edad: 25,
  saludar() { return `Hola soy ${this.nombre}`; }
};
```

### Arrays
```js
const nums = [1,2,3,4];
nums.map(n=>n*2);   // [2,4,6,8]
nums.filter(n=>n%2===0); // [2,4]
```

### Map/Set/Weak*
```js
const m = new Map();
m.set("id",123);
console.log(m.get("id"));

const s = new Set([1,2,2,3]); // {1,2,3}
```

### Copias vs referencias
```js
const a = {x:1};
const b = a;
b.x=2;
console.log(a.x); // 2
```

---

## OOP en JS/TS

### Clases, herencia, `super`
```js
class Animal {
  constructor(nombre){ this.nombre=nombre; }
  hablar(){ console.log(`${this.nombre} hace un sonido`); }
}
class Perro extends Animal {
  hablar(){ console.log(`${this.nombre} ladra`); }
}
```

### Campos `public/private/protected/readonly` (TS)
```ts
class Empleado {
  constructor(public nombre: string, private rol: string, readonly id:number){}
}
```

### `implements` y contratos
```ts
interface Notificador { enviar(msg:string): void; }
class Email implements Notificador {
  enviar(msg:string){ console.log("Email:", msg); }
}
```

---

## Asincronía

### Callbacks → Promises → `async/await`
```js
function tareaAsincrona(){
  return new Promise(res=>setTimeout(()=>res("Listo!"),1000));
}

async function run(){
  const r = await tareaAsincrona();
  console.log(r);
}
```

### Paralelismo con `Promise.all`
```js
await Promise.all([
  fetch("/api/1"),
  fetch("/api/2")
]);
```

### Errores async
```js
try {
  await fetch("/api");
} catch(e) {
  console.error("Error", e);
}
```

---

## TypeScript esencial (extra)

### Interfaces vs `type`
```ts
interface Usuario { id:number; nombre:string; }
type Rol = "ADMIN" | "USER";
```

### Utility Types (`Record`, `Partial`, …)
```ts
type Flags = Record<string, boolean>;
type User = { id:number; nombre:string; activo:boolean };
type Patch = Partial<User>;
```

### Genéricos
```ts
interface Repo<T> {
  guardar(id:number, item:T): void;
  leer(id:number): T|undefined;
}
```

### `any/unknown/never`
- `any`: desactiva el sistema de tipos (evitar).  
- `unknown`: requiere verificación antes de usar.  
- `never`: funciones que no retornan o casos imposibles.

### Enums vs literales
```ts
enum Rol { Admin="ADMIN", User="USER" }
type RolL = "ADMIN"|"USER";
```

---

## Ejercicios por sección

- Declara variables tipadas y prueba valores incorrectos.  
- Crea función que determine si un número es primo.  
- Implementa clase `Coche` con método `arrancar()`.  
- Usa `Set` para eliminar duplicados de un array.  
- Crea interfaz `Producto` y úsala en un array.  
- Implementa `Repo<T>` y pruébalo con objetos `Usuario`.  

---

## Cheat-Sheet

- `==` vs `===` → usa siempre `===`.  
- Truthy/Falsy → `0, "", null, undefined, NaN, false`.  
- Objetos se pasan **por referencia**.  
- En TS: usa `type` para composiciones, `interface` para contratos.  
- `Partial<T>` → útil para updates.  
- `Record<K,V>` → mapa tipado.  
- Funciones flecha heredan `this`.  

---

## Setup rápido (Node/TS)

```bash
mkdir fundamentos && cd fundamentos
npm init -y
npm install typescript ts-node @types/node --save-dev
npx tsc --init
```

Ejecutar un archivo TS:
```bash
npx ts-node index.ts
```
