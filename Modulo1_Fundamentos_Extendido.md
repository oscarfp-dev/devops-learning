# Módulo 1 — Fundamentos de Programación con JavaScript y TypeScript (Extendido)

> Guía extendida estilo libro. Incluye teoría, ejemplos, explicaciones detalladas, errores comunes y mejores prácticas.

---

## Índice

1. [Cómo usar esta guía](#cómo-usar-esta-guía)  
2. [JS vs TS en 2 líneas](#js-vs-ts-en-2-líneas)  
3. [Tipos en JavaScript](#tipos-en-javascript)  
4. [Tipos en TypeScript](#tipos-en-typescript)  
5. [Operadores](#operadores)  
6. [Condicionales](#condicionales)  
7. [Funciones](#funciones)  
8. [Objetos y colecciones](#objetos-y-colecciones)  
9. [Programación Orientada a Objetos (OOP)](#programación-orientada-a-objetos-oop)  
10. [Asincronía](#asincronía)  
11. [TypeScript esencial](#typescript-esencial)  
12. [Ejercicios prácticos](#ejercicios-prácticos)  
13. [Cheat-Sheet rápido](#cheat-sheet-rápido)  
14. [Setup rápido Node + TS](#setup-rápido-node--ts)

---

## Cómo usar esta guía

- Este documento es tu **manual de referencia** y también un guion para tus clases.  
- Cada sección incluye:  
  - **Código** (ejemplo práctico).  
  - **Explicaciones en bullets** (qué significa, por qué importa, errores comunes, best practices).  
- Recomendación: al exponer, **lee los bullets** y usa el código en vivo para reforzar.

---

## JS vs TS en 2 líneas

- **JavaScript (JS):**
  - Lenguaje de tipado dinámico.  
  - Corre en navegadores y Node.js.  
  - Flexible pero más propenso a errores en runtime.

- **TypeScript (TS):**
  - Superset de JS con **tipado estático**.  
  - Detecta errores en compilación.  
  - Ideal para proyectos grandes y trabajo en equipo.

**Ejemplo:**
```ts
let edad: number = 25;
edad = "25"; // ❌ Error en TS, en JS se aceptaría
```

**Explicación:**
- En JS, esta reasignación pasaría y luego fallaría en runtime.  
- En TS, se detiene antes de ejecutar → evita bugs.  
- **Best practice:** usar TS siempre que sea posible.

---

## Tipos en JavaScript

### Primitivos
```js
let texto = "Hola";      // string
let numero = 123;        // number (enteros y decimales)
let activo = true;       // boolean
let nada = null;         // null
let indefinido;          // undefined
let big = 10n;           // bigint
let sym = Symbol("id");  // symbol
```

**Explicación:**
- Estos son **inmutables**: no puedes cambiar su valor interno, solo reasignar.  
- `null` = valor intencionalmente vacío.  
- `undefined` = variable declarada pero sin valor.  
- `bigint` = enteros muy grandes, útiles para cálculos financieros o criptográficos.  
- `symbol` = identificador único, útil en metaprogramación.  

**Errores comunes:**
- Usar `null` y `undefined` como si fueran lo mismo.  
- Intentar sumar `bigint` con `number` (lanza error).  

**Best practice:** siempre inicializar variables para evitar `undefined`.

---

### Objetos
```js
const usuario = {
  id: 1,
  nombre: "Ana",
  activo: true
};
```

**Explicación:**
- Objetos son **colecciones de pares clave–valor**.  
- Las claves son strings (o symbols).  
- Se usan para representar entidades.  

**Errores comunes:**
- Acceder a propiedades que no existen (`usuario.edad` → undefined).  
- Modificar objetos sin querer por referencia.  

**Best practice:** usar `const` y crear copias (`{...obj}`) para evitar mutaciones no deseadas.

---

### `typeof` y Truthy/Falsy
```js
typeof "hola"; // "string"
typeof null;   // "object" (quirk histórico)
```

**Valores falsy:** `0`, `""`, `null`, `undefined`, `NaN`, `false`.  
Todo lo demás es truthy.

```js
if ("") console.log("no");     // no entra
if ("hola") console.log("sí"); // sí entra
```

**Explicación:**
- Importante al validar datos de usuario.  
- Puede provocar errores si no entiendes qué es falsy.  

**Best practice:** usar comparaciones explícitas (`=== null`, `=== undefined`) en lugar de confiar en truthy/falsy.

---

## Tipos en TypeScript

### Anotaciones e inferencia
```ts
let nombre: string = "Ana";
let edad: number = 30;
let activo: boolean = true;

let ciudad = "CDMX"; // inferencia: TS sabe que es string
```

**Explicación:**
- **Anotación:** defines el tipo explícitamente.  
- **Inferencia:** TS deduce el tipo.  
- Ambas son válidas, pero las anotaciones ayudan a documentar.  

**Best practice:** usar anotaciones en funciones públicas y límites de módulos.

---

### Uniones e intersecciones
```ts
type Estado = "pendiente" | "hecha"; // unión literal
let e: Estado = "pendiente";

type Base = { id: number };
type Avanzado = { permisos: string[] };
type Usuario = Base & Avanzado; // intersección
```

**Explicación:**
- Uniones → un valor puede ser de varios tipos.  
- Intersecciones → combinar varios tipos en uno.  
- Muy útil para contratos de datos.  

---

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

**Explicación:**
- TS requiere que “estreches” (`narrow`) el tipo antes de usarlo.  
- Esto evita llamar métodos que no existen para un tipo.  

---

## Operadores

### Comparación
```js
0 == false   // true  (coerción)
0 === false  // false (estricto)
"5" == 5     // true
"5" === 5    // false
```

**Explicación:**
- `==` hace conversión automática → resultados inesperados.  
- `===` compara tipo + valor → recomendado.  

**Best practice:** siempre usar `===` salvo casos muy concretos.

---

### Lógicos, `??`, `?.`
```js
const nombre = "" || "Anónimo";   // "Anónimo"
const activo = true && "OK";      // "OK"
const valor = undefined ?? "def"; // "def"

const obj = { user: { id: 1 } };
console.log(obj?.user?.id); // 1
```

**Explicación:**
- `||` devuelve el primer truthy.  
- `&&` devuelve el último si todo es truthy.  
- `??` solo actúa con `null` o `undefined`.  
- `?.` evita errores al acceder a propiedades inexistentes.  

---

### Spread/Rest
```js
const arr = [1,2,3];
const copia = [...arr, 4];

function suma(...nums) {
  return nums.reduce((a,b)=>a+b,0);
}
```

**Explicación:**
- Spread (`...arr`) copia/expande.  
- Rest (`...nums`) agrupa en array.  

**Errores comunes:** confundir `...` en estos dos contextos.

---

## Condicionales

```js
if (edad >= 18) {
  console.log("Mayor de edad");
} else {
  console.log("Menor de edad");
}

const estado = activo ? "ON" : "OFF";
```

**Explicación:**
- If/else → control clásico.  
- Ternario → forma compacta, útil en expresiones cortas.  

**Best practice:** usar guard clauses para evitar `if` anidados.

---

## Funciones

### Normales vs flecha
```js
function normal(a,b){ return a+b; }
const flecha = (a,b) => a+b;
```

**Explicación:**
- Normales tienen su propio `this` y `arguments`.  
- Flechas heredan `this` del contexto y no tienen `arguments`.  
- Flechas no sirven como constructores.  

---

### Ejemplo con `this`
```js
const obj = {
  valor: 42,
  normal: function(){ setTimeout(function(){ console.log(this.valor) },0); },
  flecha: function(){ setTimeout(()=>console.log(this.valor),0); }
};
```

**Explicación:**
- En `normal`, `this` apunta a `window`/`global` → undefined.  
- En `flecha`, `this` apunta al objeto `obj`.  

**Best practice:** usar flechas en callbacks.

---

## Objetos y colecciones

### Array
```js
const nums = [1,2,3,4];
nums.map(n=>n*2);   // [2,4,6,8]
nums.filter(n=>n%2===0); // [2,4]
```

**Explicación:**
- Arrays son muy potentes con métodos funcionales (`map`, `filter`, `reduce`).  
- **Error común:** olvidar que métodos como `map` devuelven un **nuevo array** (no modifican el original).  

---

### Map y Set
```js
const m = new Map();
m.set("id",123);
console.log(m.get("id"));

const s = new Set([1,2,2,3]); // {1,2,3}
```

**Explicación:**
- Map → claves de cualquier tipo, mejor que objeto en algunos casos.  
- Set → colección sin duplicados.  
- **Error común:** usar objeto `{}` como diccionario cuando necesitas claves que no son strings.  

---

## Programación Orientada a Objetos (OOP)

```js
class Persona {
  constructor(nombre, edad){
    this.nombre = nombre;
    this.edad = edad;
  }
  saludar(){ console.log(`Hola soy ${this.nombre}`); }
}
```

**Explicación:**
- `class` es azúcar sintáctico sobre prototipos de JS.  
- Métodos definidos dentro van al prototipo.  
- TS añade modificadores (`public`, `private`, `protected`, `readonly`).  

---

## Asincronía

```js
function tareaAsincrona(){
  return new Promise(res=>setTimeout(()=>res("Listo!"),1000));
}

async function run(){
  const r = await tareaAsincrona();
  console.log(r);
}
```

**Explicación:**
- Promises representan valores futuros.  
- `async/await` simplifica el manejo, parece código síncrono.  
- **Error común:** olvidar `await` y obtener `Promise {}` en consola.  

---

## TypeScript esencial

### Interfaces vs type
```ts
interface Usuario { id:number; nombre:string; }
type Rol = "ADMIN" | "USER";
```

**Explicación:**
- Interface → contratos, extendibles.  
- Type → composiciones, uniones, literales.  
- Ambos son compatibles, elige según necesidad.  

---

### Utility Types
```ts
type User = { id:number; nombre:string; activo:boolean };
type Patch = Partial<User>;
type Flags = Record<string, boolean>;
```

**Explicación:**
- Partial → vuelve todas las props opcionales.  
- Record → objeto con claves/valores tipados.  
- Muy usados en APIs y bases de datos.  

---

### Genéricos
```ts
interface Repo<T> {
  guardar(id:number, item:T): void;
  leer(id:number): T|undefined;
}
```

**Explicación:**
- Genéricos permiten crear estructuras reutilizables.  
- Evitan repetir lógica para cada tipo de dato.  

---

### any / unknown / never
- `any`: evita el chequeo de tipos → último recurso.  
- `unknown`: más seguro, requiere comprobación antes de usar.  
- `never`: funciones que no retornan o casos imposibles.  

---

## Ejercicios prácticos

- Declara variables tipadas y prueba valores incorrectos.  
- Crea función que determine si un número es primo.  
- Implementa clase `Coche` con método `arrancar()`.  
- Usa `Set` para eliminar duplicados de un array.  
- Crea interfaz `Producto` y úsala en un array.  
- Implementa `Repo<T>` y pruébalo con objetos `Usuario`.  

---

## Cheat-Sheet rápido

- `===` siempre mejor que `==`.  
- Valores falsy: `0, "", null, undefined, NaN, false`.  
- Objetos se pasan **por referencia**.  
- En TS: usa `interface` para contratos y `type` para combinaciones.  
- `Partial<T>` para updates parciales.  
- Funciones flecha heredan `this`.  

---

## Setup rápido Node + TS

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
