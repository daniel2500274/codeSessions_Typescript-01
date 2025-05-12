# TypeScript y Node.js - Guía Rápida de Sintaxis

Es una guía de referencia rápida para la sintaxis de TypeScript y conceptos de Node.js comúnmente utilizados en los proyectos de aprendizaje.

## 1. Configuración Básica del Proyecto (Node.js + TypeScript)

### `package.json`
Archivo que define el proyecto, sus dependencias y scripts. Se crea con `npm init -y`.

```json
{
  "name": "mi-proyecto",
  "version": "1.0.0",
  "description": "",
  "main": "dist/index.js", // Archivo de entrada principal (JavaScript compilado)
  "scripts": {
    "build": "tsc",
    "start": "node dist/index.js", // O "tsc && node dist/index.js"
    "dev": "ts-node-dev --respawn src/index.ts" // Opcional, para desarrollo
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "@types/node": "^20.0.0", // Tipos para Node.js
    "typescript": "^5.0.0"   // Compilador de TypeScript
  },
  "dependencies": {
    // Librerías de producción, ej: "readline-sync": "^1.4.10"
  }
}
```
* **`main`**: Punto de entrada de la aplicación (usualmente el compilado).
* **`scripts`**: Comandos para automatizar tareas (compilar, ejecutar).
* **`devDependencies`**: Paquetes para desarrollo (TypeScript, tipos).
* **`dependencies`**: Paquetes que la aplicación necesita en producción.

### `tsconfig.json`
Archivo de configuración para el compilador de TypeScript (`tsc`). Se crea con `npx tsc --init`.

```json
{
  "compilerOptions": {
    "target": "ES2020",             // Versión de ECMAScript de salida
    "module": "commonjs",           // Sistema de módulos (Node.js usa commonjs)
                                    // o "ESNext" para módulos ES6 si configuras Node para ello
    "rootDir": "./src",             // Directorio raíz de los archivos TypeScript
    "outDir": "./dist",             // Directorio de salida para los archivos JavaScript compilados
    "strict": true,                 // Habilita todas las opciones de chequeo estricto
    "esModuleInterop": true,        // Permite interoperabilidad con módulos CommonJS
    "skipLibCheck": true,           // Omite la verificación de tipos de los archivos de declaración
    "forceConsistentCasingInFileNames": true // Asegura consistencia en mayúsculas/minúsculas de nombres de archivo
  },
  "include": ["src/**/*"],          // Qué archivos incluir en la compilación
  "exclude": ["node_modules", "dist"] // Qué archivos excluir
}
```
* **`target`**: Especifica la versión de ECMAScript a la que se compilará el código.
* **`module`**: Define el sistema de módulos. `commonjs` es típico para Node.js.
* **`rootDir`**: Carpeta donde residen los archivos `.ts`.
* **`outDir`**: Carpeta donde se guardarán los archivos `.js` compilados.
* **`strict`**: Habilita un conjunto de verificaciones de tipo más rigurosas.

## 2. Variables y Tipos Básicos (TypeScript)

### Declaración de Variables (`let`, `const`) con Tipos
TypeScript permite especificar el tipo de una variable.

```typescript
// let: para variables que pueden cambiar su valor
let mensaje: string = "Hola Mundo";
let edad: number = 30;
let esActivo: boolean = true;

// const: para variables cuyo valor no cambiará (constantes)
const PI: number = 3.1416;
const URL_API: string = "[https://api.ejemplo.com](https://api.ejemplo.com)";
```
* **`let`**: Declara una variable cuyo valor puede ser reasignado.
* **`const`**: Declara una constante cuyo valor no puede ser reasignado una vez inicializado.
* **Tipos explícitos (`: string`, `: number`, `: boolean`)**: Aunque TypeScript puede inferir tipos, es buena práctica ser explícito.

### Tipos Primitivos
Son los bloques de construcción básicos para los datos.

```typescript
let texto: string = "Esto es un texto";
let numeroEntero: number = 100;
let numeroDecimal: number = 10.5;
let verdadero: boolean = true;
let falso: boolean = false;
```

### Otros Tipos Comunes
```typescript
let cualquiera: any = "Puede ser cualquier cosa"; // Evitar su uso excesivo
cualquiera = 5;
cualquiera = false;

function saludar(): void { // 'void' para funciones que no retornan valor
  console.log("Hola!");
}

let nulo: null = null;
let indefinido: undefined = undefined;
```
* **`any`**: Un tipo dinámico que puede representar cualquier valor. Usar con precaución ya que deshabilita la verificación de tipos.
* **`void`**: Representa la ausencia de un valor, comúnmente usado como tipo de retorno de funciones que no devuelven nada.
* **`null` y `undefined`**: Representan la ausencia intencional de un valor y un valor no asignado, respectivamente.

## 3. Estructuras de Datos (TypeScript)

### Arreglos (`Array`)
Colecciones ordenadas de elementos del mismo tipo (o de tipos mixtos si se usa `any` o uniones).

```typescript
// Sintaxis recomendada
let numeros: number[] = [1, 2, 3, 4, 5];
let nombres: string[] = ["Ana", "Luis", "Eva"];
let booleanos: boolean[] = [true, false, true];

// Sintaxis genérica (menos común para tipos simples)
let otrosNumeros: Array<number> = [10, 20, 30];

// Arreglo de cualquier tipo (usar con precaución)
let mezcla: any[] = [1, "dos", true, { id: 3 }];
```
* Se pueden declarar usando `tipo[]` o `Array<tipo>`.
* Los elementos se acceden por su índice (base 0): `numeros[0]`.

### Enums (`enum`)
Permiten definir un conjunto de constantes nombradas. Son útiles para representar un conjunto finito de opciones.

```typescript
enum Color {
  Rojo,    // Por defecto, Rojo = 0
  Verde,   // Verde = 1
  Azul     // Azul = 2
}

let miColor: Color = Color.Verde; // miColor tendrá el valor 1
console.log(miColor); // Imprime: 1
console.log(Color[1]); // Imprime: Verde

enum EstadoPedido {
  Pendiente = "PENDIENTE",
  Procesando = "PROCESANDO",
  Enviado = "ENVIADO",
  Entregado = "ENTREGADO"
}

let pedidoActual: EstadoPedido = EstadoPedido.Procesando;
console.log(pedidoActual); // Imprime: "PROCESANDO"
```
* **Numéricos**: Por defecto, los enums asignan valores numéricos incrementales empezando desde 0.
* **Basados en Strings**: Se pueden asignar valores de string explícitamente, lo que puede ser más legible.
* **NO llevan `()` en su declaración.**

## 4. Definición de Formas de Datos (TypeScript)

### Interfaces
Definen un "contrato" para la forma que deben tener los objetos. Son una poderosa forma de definir estructuras de datos.

```typescript
interface Usuario {
  id: number;
  nombre: string;
  email: string;
  estaActivo?: boolean; // Propiedad opcional con '?'
  readonly pais: string; // Propiedad de solo lectura
}

let unUsuario: Usuario = {
  id: 1,
  nombre: "Carlos",
  email: "carlos@ejemplo.com",
  pais: "Guatemala"
  // estaActivo es opcional
};

// unUsuario.pais = "México"; // Error: pais es readonly

interface Calculadora {
  sumar(a: number, b: number): number;
  restar: (a: number, b: number) => number; // Otra forma de definir métodos
}

class MiCalculadora implements Calculadora {
  sumar(a: number, b: number): number {
    return a + b;
  }
  restar(a: number, b: number): number {
    return a - b;
  }
}
```
* Definen la estructura (propiedades y sus tipos, métodos y sus firmas) que un objeto debe cumplir.
* Las propiedades pueden ser opcionales (`?`) o de solo lectura (`readonly`).
* Las clases pueden `implementar` interfaces para asegurar que cumplen con el contrato.

### Type Aliases (`type`)
Permiten crear un nuevo nombre (alias) para un tipo existente. Útil para tipos complejos, uniones, tuplas, etc.

```typescript
type Identificador = string | number; // Tipo unión
type Punto = { x: number; y: number }; // Alias para un tipo objeto
type EstadoCivil = "Soltero" | "Casado" | "Divorciado" | "Viudo"; // Tipo literal unión

let userId: Identificador = "abc-123";
userId = 456;

let coordenada: Punto = { x: 10, y: 20 };
let miEstado: EstadoCivil = "Casado";

// También para funciones
type FuncionDeSuma = (val1: number, val2: number) => number;
const sumarValores: FuncionDeSuma = (a, b) => a + b;
```
* Similares a las interfaces para definir formas de objetos, pero más flexibles para otros tipos como uniones o primitivos.
* Interfaces pueden ser extendidas (`extends`) y clases pueden implementarlas (`implements`). Type aliases no pueden ser extendidos o implementados de la misma forma (pero se pueden lograr efectos similares con intersecciones `&`).

### DTOs (Data Transfer Objects)
Son objetos simples cuyo propósito es transferir datos entre capas o módulos de una aplicación. A menudo se definen usando interfaces o `type aliases`.

```typescript
// DTO para crear un producto, omite el 'id' que se genera después
interface CrearProductoDTO {
  nombre: string;
  precio: number;
  categoria: string;
  stockInicial?: number;
}

function agregarProducto(datos: CrearProductoDTO) {
  // Lógica para crear el producto con un ID generado internamente
  const nuevoProducto = { id: Math.random(), ...datos };
  console.log("Producto agregado:", nuevoProducto);
}

agregarProducto({ nombre: "Laptop", precio: 1200, categoria: "Electrónica" });
```

## 5. Funciones (TypeScript)

### Definición con Tipos en Parámetros y Retorno
TypeScript permite (y recomienda) tipar los parámetros de las funciones y su valor de retorno.

```typescript
function sumar(a: number, b: number): number {
  return a + b;
}

function imprimirMensaje(mensaje: string, veces?: number): void { // Parámetro opcional 'veces'
  const numVeces = veces || 1; // Valor por defecto si 'veces' es undefined
  for (let i = 0; i < numVeces; i++) {
    console.log(mensaje);
  }
}

// Parámetros con valores por defecto
function crearUsuario(nombre: string, rol: string = "Usuario"): { nombre: string; rol: string } {
  return { nombre, rol };
}

let resultadoSuma: number = sumar(5, 3); // 8
imprimirMensaje("Hola TypeScript");
imprimirMensaje("Adiós", 3);
let nuevoUsuario = crearUsuario("Ana"); // rol será "Usuario"
```
* **Parámetros opcionales (`?`)**: Deben ir después de los parámetros requeridos.
* **Parámetros por defecto (`rol: string = "Usuario"`)**: Si no se provee un argumento, se usa el valor por defecto.
* **Tipo de retorno `void`**: Para funciones que no devuelven ningún valor.

### Funciones Flecha (`Arrow Functions`)
Una sintaxis más concisa para definir funciones.

```typescript
const restar = (a: number, b: number): number => {
  return a - b;
};

const multiplicar = (a: number, b: number): number => a * b; // Retorno implícito si es una sola expresión

const saludarUsuario = (nombre: string): void => console.log(`Hola, ${nombre}`);

restar(10, 4); // 6
multiplicar(5, 5); // 25
saludarUsuario("Juan");
```
* Si la función tiene una sola expresión como cuerpo y retorna su resultado, se pueden omitir las llaves `{}` y la palabra `return`.

## 6. Clases (TypeScript)

### Definición Básica
Plantillas para crear objetos. Encapsulan datos (propiedades) y comportamiento (métodos).

```typescript
class Persona {
  // Propiedades (atributos)
  nombre: string;
  edad: number;
  private _identificacionSecreta: string; // Propiedad privada

  // Constructor: se ejecuta al crear una instancia (new Persona(...))
  constructor(nombreInicial: string, edadInicial: number, idSecreta: string) {
    this.nombre = nombreInicial;
    this.edad = edadInicial;
    this._identificacionSecreta = idSecreta;
    console.log(`Persona ${this.nombre} creada.`);
  }

  // Métodos (comportamiento)
  saludar(): void {
    console.log(`Hola, mi nombre es ${this.nombre} y tengo ${this.edad} años.`);
  }

  cumplirAnios(): void {
    this.edad++;
  }

  // Getter para propiedad privada
  get identificacion(): string {
      return `ID Oculto: ${this._identificacionSecreta.substring(0,3)}***`;
  }

  // Setter para propiedad privada
  set nuevaIdentificacion(nuevaId: string) {
      if (nuevaId.length > 5) {
          this._identificacionSecreta = nuevaId;
      } else {
          console.error("La identificación debe tener más de 5 caracteres.");
      }
  }
}

// Instanciación de la clase
let persona1: Persona = new Persona("Elena", 28, "XYZ12345");
persona1.saludar(); // Hola, mi nombre es Elena y tengo 28 años.
persona1.cumplirAnios();
console.log(persona1.edad); // 29
// console.log(persona1._identificacionSecreta); // Error: _identificacionSecreta es privada
console.log(persona1.identificacion); // Accede mediante el getter
persona1.nuevaIdentificacion = "ABCDEFGH";
console.log(persona1.identificacion);
```

### Modificadores de Acceso
Controlan la visibilidad de las propiedades y métodos de una clase.
* **`public` (por defecto)**: Accesible desde cualquier lugar.
* **`private`**: Accesible solo desde dentro de la misma clase.
* **`protected`**: Accesible desde dentro de la misma clase y desde clases que heredan de ella.

```typescript
class Animal {
  public nombre: string;
  private edad: number; // Solo accesible dentro de Animal
  protected tipo: string; // Accesible en Animal y clases hijas

  constructor(nombre: string, edad: number, tipo: string) {
    this.nombre = nombre;
    this.edad = edad;
    this.tipo = tipo;
  }

  public hacerSonido(): void {
    console.log("El animal hace un sonido.");
  }

  public getEdad(): number {
      return this.edad;
  }
}

class Perro extends Animal {
  constructor(nombre: string, edad: number) {
    super(nombre, edad, "Mamífero"); // Llama al constructor de la clase padre
  }

  public ladrar(): void {
    console.log("Guau guau!");
    console.log(`Soy un ${this.tipo}`); // 'tipo' es accesible porque es protected
    // console.log(this.edad); // Error: 'edad' es privada en Animal
    console.log(`Tengo ${this.getEdad()} años.`); // Accede a edad a través de un método público
  }
}

let miPerro = new Perro("Fido", 5);
miPerro.ladrar();
// miPerro.edad; // Error
// miPerro.tipo; // Error: tipo es protected y se accede desde fuera de la jerarquía
```

### Propiedades `readonly`
Propiedades cuyo valor solo se puede asignar en el constructor o en su declaración.

```typescript
class Vehiculo {
  readonly numeroSerie: string;
  marca: string;

  constructor(numeroSerie: string, marca: string) {
    this.numeroSerie = numeroSerie; // Se asigna aquí
    this.marca = marca;
  }

  cambiarMarca(nuevaMarca: string) {
    this.marca = nuevaMarca;
    // this.numeroSerie = "otro"; // Error: numeroSerie es readonly
  }
}
```

### Shorthand para Constructor (Inicialización de Propiedades)
TypeScript permite declarar e inicializar propiedades directamente en los parámetros del constructor usando modificadores de acceso.

```typescript
class Producto {
  // public nombre: string;
  // private precio: number;
  // readonly id: string;

  constructor(
    public nombre: string,
    private precio: number,
    readonly id: string
  ) {
    // No se necesita this.nombre = nombre;, etc. TypeScript lo hace automáticamente.
  }

  getPrecio(): number {
    return this.precio;
  }

  display(): void {
    console.log(`ID: ${this.id}, Producto: ${this.nombre}, Precio: $${this.precio}`);
  }
}

const prod = new Producto("Teclado", 50, "TEC-001");
prod.display();
console.log(prod.nombre); // Accesible
// console.log(prod.precio); // Error: precio es privado
console.log(prod.getPrecio());
```

## 7. Módulos (TypeScript/ES6)

Permiten organizar el código en archivos separados y reutilizables.

### `export`
Hace que las declaraciones (variables, funciones, clases, interfaces, enums, etc.) estén disponibles para otros módulos.

```typescript
// en utils.ts
export const PI: number = 3.14159;

export function calcularAreaCirculo(radio: number): number {
  return PI * radio * radio;
}

export interface Figura {
  calcularArea(): number;
}

export class Circulo implements Figura {
    constructor(public radio: number) {}
    calcularArea(): number {
        return PI * this.radio * this.radio;
    }
}

// Export por defecto (solo uno por archivo)
// export default class MiClasePrincipal { ... }
```

### `import`
Trae declaraciones exportadas de otros módulos al ámbito actual.

```typescript
// en main.ts
import { PI, calcularAreaCirculo, Figura, Circulo } from './utils';
// Si hubiera un export default en utils.ts:
// import MiClasePrincipal from './utils';

console.log(PI); // 3.14159
console.log(calcularAreaCirculo(10));

const miCirculo: Figura = new Circulo(5);
console.log(`Área del círculo: ${miCirculo.calcularArea()}`);
```
* Usar rutas relativas (`./`, `../`) para importar módulos locales.

## 8. Operadores y Lógica Común (JavaScript/TypeScript)

### Ciclos
```typescript
let lista: number[] = [10, 20, 30];

// for...of (para iterar sobre valores de un iterable)
for (const item of lista) {
  console.log(item);
}

// while
let contador = 0;
while (contador < 3) {
  console.log(`Contador: ${contador}`);
  contador++;
}
```

### Métodos de Arreglos Comunes
```typescript
let items: string[] = ["a", "b", "c"];
let numeros: number[] = [1, 2, 3, 4, 5];

// forEach: Ejecuta una función por cada elemento
items.forEach(item => console.log(item.toUpperCase()));

// map: Crea un nuevo arreglo con los resultados de llamar una función en cada elemento
let numerosDobles: number[] = numeros.map(n => n * 2); // [2, 4, 6, 8, 10]

// filter: Crea un nuevo arreglo con elementos que cumplen una condición
let numerosPares: number[] = numeros.filter(n => n % 2 === 0); // [2, 4]

// find: Devuelve el primer elemento que cumple una condición, o undefined
let primerPar: number | undefined = numeros.find(n => n % 2 === 0); // 2

// findIndex: Devuelve el índice del primer elemento que cumple una condición, o -1
let indicePrimerPar: number = numeros.findIndex(n => n % 2 === 0); // 1

// splice: Cambia el contenido de un arreglo eliminando, reemplazando o agregando elementos
let colores: string[] = ["rojo", "verde", "azul", "amarillo"];
// Eliminar "verde" (índice 1, 1 elemento)
let eliminados = colores.splice(1, 1); // eliminados = ["verde"], colores = ["rojo", "azul", "amarillo"]
// Reemplazar "azul" por "morado" y "naranja"
colores.splice(1, 1, "morado", "naranja"); // colores = ["rojo", "morado", "naranja", "amarillo"]

// some: Verifica si al menos un elemento cumple una condición
let hayAlgunPar: boolean = numeros.some(n => n % 2 === 0); // true
```

### Condicionales
```typescript
let edadPermitida: number = 18;
let edadUsuario: number = 20;

if (edadUsuario >= edadPermitida) {
  console.log("Puede entrar.");
} else {
  console.log("No puede entrar.");
}

let dia: string = "Lunes";
switch (dia) {
  case "Lunes":
    console.log("Inicio de semana.");
    break;
  case "Viernes":
    console.log("Casi fin de semana!");
    break;
  default:
    console.log("Otro día.");
}
```

### Template Literals (Plantillas de Cadenas)
Permiten incrustar expresiones dentro de cadenas de texto.

```typescript
let nombreUsuario: string = "Ana";
let puntos: number = 100;
let bienvenida: string = `Hola ${nombreUsuario}, tienes ${puntos} puntos.`;
console.log(bienvenida); // Hola Ana, tienes 100 puntos.
```

### Spread & Rest Operators (`...`)
* **Spread**: Expande un iterable (como un arreglo o string) en lugares donde se esperan cero o más argumentos o elementos.
* **Rest**: Agrupa el "resto" de los parámetros de una función en un arreglo.

```typescript
// Spread
let arr1: number[] = [1, 2];
let arr2: number[] = [...arr1, 3, 4]; // arr2 = [1, 2, 3, 4]

let obj1 = { a: 1, b: 2 };
let obj2 = { ...obj1, c: 3 }; // obj2 = { a: 1, b: 2, c: 3 }

// Rest
function sumarTodos(...numeros: number[]): number {
  return numeros.reduce((acc, current) => acc + current, 0);
}
console.log(sumarTodos(1, 2, 3)); // 6
console.log(sumarTodos(10, 20, 30, 40)); // 100
```

## 9. Interacción y Módulos Node.js (Ejemplos)

### `console.log()`
Función global en Node.js (y navegadores) para imprimir mensajes en la consola.

```typescript
console.log("Mensaje informativo");
console.warn("Advertencia");
console.error("Esto es un error");

let objeto = { id: 1, valor: "test" };
console.log("Datos del objeto:", objeto);
```

### `readline-sync` (Librería Externa)
Permite leer la entrada del usuario de forma síncrona en la consola.
Primero, instalar: `npm install readline-sync @types/readline-sync`

```typescript
// Al inicio de tu archivo .ts
import * as readlineSync from 'readline-sync';

// Preguntar texto
const nombre: string = readlineSync.question('Cual es tu nombre? ');
console.log(`Hola, ${nombre}!`);

// Preguntar con opciones (selección)
const animales: string[] = ['Perro', 'Gato', 'Pajaro'];
const indice: number = readlineSync.keyInSelect(animales, 'Cual es tu animal favorito?');

if (indice !== -1) { // -1 si el usuario cancela (ej. presionando ESC)
  console.log(`Tu animal favorito es ${animales[indice]}.`);
} else {
  console.log('No seleccionaste un animal.');
}

// Confirmación (Sí/No)
if (readlineSync.keyInYN('Estas seguro?')) {
  console.log('Procesando...');
} else {
  console.log('Cancelado.');
}
```

### `fs` (Sistema de Archivos - Módulo de Node.js)
Permite interactuar con el sistema de archivos. (Ejemplo síncrono, para simplicidad).
*Nota: En aplicaciones más grandes, se prefieren las versiones asíncronas para no bloquear el hilo principal.*

```typescript
import * as fs from 'fs'; // Importar el módulo fs
import * as path from 'path'; // Útil para construir rutas de archivo

const nombreArchivo = 'miArchivo.txt';
const rutaCompleta = path.join(__dirname, nombreArchivo); // __dirname es el directorio del archivo actual

// Escribir en un archivo (síncrono)
try {
  const contenido = 'Hola desde Node.js y TypeScript!';
  fs.writeFileSync(rutaCompleta, contenido, 'utf8');
  console.log(`Archivo "${nombreArchivo}" escrito exitosamente.`);
} catch (error) {
  console.error('Error al escribir el archivo:', error);
}

// Leer desde un archivo (síncrono)
try {
  const datosLeidos: string = fs.readFileSync(rutaCompleta, 'utf8');
  console.log(`Contenido de "${nombreArchivo}": ${datosLeidos}`);
} catch (error) {
  console.error('Error al leer el archivo:', error);
}
```

## 10. Tipos Utilitarios de TypeScript (Ejemplos)

TypeScript provee tipos utilitarios que ayudan a transformar tipos existentes.

### `Partial<T>`
Construye un tipo con todas las propiedades de `T` establecidas como opcionales.

```typescript
interface Tarea {
  id: number;
  titulo: string;
  descripcion: string;
  completada: boolean;
}

// Permite actualizar solo algunas propiedades de una Tarea
function actualizarTarea(id: number, actualizacion: Partial<Tarea>): void {
  // Lógica para encontrar la tarea con 'id' y aplicar 'actualizacion'
  console.log(`Actualizando tarea ${id} con:`, actualizacion);
  // tareaOriginal.titulo = actualizacion.titulo || tareaOriginal.titulo; (ejemplo)
}

actualizarTarea(1, { descripcion: "Nueva descripción", completada: true });
// También válido: actualizarTarea(2, { titulo: "Otro título" });
```

### `Omit<T, K>`
Construye un tipo seleccionando todas las propiedades de `T` y luego eliminando las claves `K`.

```typescript
interface PerfilUsuario {
  id: string;
  nombre: string;
  email: string;
  contrasena: string; // Queremos omitir esto para el DTO público
  fechaCreacion: Date;
}

// DTO para mostrar un perfil sin la contraseña
type PerfilPublicoDTO = Omit<PerfilUsuario, 'contrasena'>;

function obtenerPerfilPublico(id: string): PerfilPublicoDTO {
  // Lógica para obtener el usuario completo
  const usuarioCompleto: PerfilUsuario = {
      id,
      nombre: "Usuario Ejemplo",
      email: "ej@mp.lo",
      contrasena: "secreto123",
      fechaCreacion: new Date()
  };

  // Omitimos la contraseña antes de retornarlo
  const { contrasena, ...perfilPublico } = usuarioCompleto;
  return perfilPublico;
}

const perfil = obtenerPerfilPublico("usr-1");
console.log(perfil.nombre);
// console.log(perfil.contrasena); // Error: la propiedad 'contrasena' no existe en PerfilPublicoDTO
```
---

