# Requerimientos

## Configuración del Proyecto:

-   Crea un nuevo directorio para tu proyecto (ej. `gestor-inventario`).
-   Dentro, ejecuta `npm init -y` para crear un `package.json`.
-   Instala TypeScript y los tipos para Node:
    ```bash
    npm install typescript @types/node --save-dev
    ```
-   Genera un archivo `tsconfig.json` base:
    ```bash
    npx tsc --init
    ```
    -   *Pista: Modifica `tsconfig.json` para que `outDir` sea `"./dist"` y `rootDir` sea `"./src"`. Esto organiza tu código compilado y fuente.*
-   Crea un directorio `src` y dentro un archivo `index.ts`.
-   En `package.json`, añade un script para ejecutar tu proyecto: `"start": "tsc && node dist/index.js"`.

## Definir el Enum para Categorías:

-   En un archivo `src/enums.ts` (o directamente en `src/index.ts` si prefieres), define un enum llamado `CategoriaProducto`.
-   Valores sugeridos: `Electronica`, `Ropa`, `Alimentos`, `Hogar`, `Juguetes`.
-   *Pista: Los enum te ayudan a tener un conjunto fijo de opciones, lo que previene errores de escritura y hace tu código más legible.*
    ```typescript
    export enum CategoriaProducto {
        Electronica,
        Ropa,
        Alimentos,
        Hogar,
        Juguetes
    }
    ```

## Definir la Interfaz Producto:

-   En un archivo `src/interfaces.ts` (o en `src/index.ts`), crea una interfaz `Producto`.
-   Propiedades:
    -   `id: number` (debe ser único)
    -   `nombre: string`
    -   `descripcion: string`
    -   `precio: number`
    -   `stock: number`
    -   `categoria: CategoriaProducto`
-   *Pista: `export interface Producto { ... }`. Esta interfaz será el "contrato" de cómo deben lucir tus objetos de producto.*

## Definir el DTO CrearProductoDTO:

-   En el mismo archivo de interfaces, crea un tipo o interfaz `CrearProductoDTO` (Data Transfer Object).
-   Este DTO debe tener las propiedades necesarias para crear un nuevo producto. Puede omitir el `id` (que se generará automáticamente) pero incluir `nombre`, `descripcion`, `precio`, `stock` inicial y `categoria`.
-   *Pista: `export type CrearProductoDTO = Omit<Producto, 'id'>;` o define las propiedades explícitamente. Los DTO son útiles para definir la forma de los datos que se mueven entre partes de tu aplicación, como los datos que un método espera recibir.*

## Crear la Clase GestorInventario:

-   En un nuevo archivo `src/gestorInventario.ts`, define y exporta una clase `GestorInventario`.
-   Debe tener una propiedad privada `productos: Producto[] = [];`.
-   Añade una propiedad privada `siguienteId: number = 1;` para ayudarte a generar IDs únicos para los productos.

### Método `agregarProducto(datosProducto: CrearProductoDTO): Producto`:

-   Dentro de `GestorInventario`, crea este método público.
-   Debe tomar un `CrearProductoDTO` como argumento.
-   Dentro del método, crea un nuevo objeto `Producto` completo:
    -   Asigna un `id` usando `this.siguienteId` y luego incrementa `this.siguienteId`.
    -   Copia las demás propiedades desde `datosProducto`.
-   Agrega este nuevo producto al arreglo `this.productos`.
-   Retorna el producto recién creado.
-   *Pista: `const nuevoProducto: Producto = { id: this.siguienteId++, ...datosProducto };`.*

### Método `listarProductos(): void`:

-   Este método no recibe argumentos.
-   Debe recorrer el arreglo `this.productos`.
-   Para cada producto, imprime en la consola sus detalles de forma clara (ej: `ID: 1, Nombre: Laptop, Precio: $1200, Stock: 10, Categoría: Electronica`).
-   *Pista: Puedes usar un bucle `for...of` o el método `forEach` de los arreglos. `console.log()`.

### Método `buscarProductoPorId(id: number): Producto | undefined`:

-   Recibe un `id` numérico.
-   Busca en `this.productos` un producto cuyo `id` coincida.
-   Si lo encuentra, retorna el objeto `Producto`. Si no, retorna `undefined`.
-   *Pista: El método `find()` de los arreglos es perfecto para esto. `return this.productos.find(p => p.id === id);`.*

### Método `actualizarStock(idProducto: number, cantidadCompradaOVendida: number, tipo: 'entrada' | 'salida'): boolean`:

-   Recibe el `idProducto`, la `cantidad` y un `tipo` que indique si es una `'entrada'` (aumentar stock) o `'salida'` (disminuir stock).
-   Busca el producto por su `id`.
-   Si el producto existe:
    -   Si es `'entrada'`, suma la cantidad al stock.
    -   Si es `'salida'`, resta la cantidad del stock. Asegúrate de que el stock no quede negativo (si es así, quizás no actualices y devuelvas `false`, o lances un error).
-   Retorna `true` si la actualización fue exitosa.
-   Si el producto no existe, retorna `false`.
-   *Pista: Primero encuentra el producto. Luego, aplica la lógica condicional para el stock.*

## Instanciación y Pruebas (en `src/index.ts`):

-   Importa `GestorInventario`, `CategoriaProducto` y tu `CrearProductoDTO`.
-   Crea una instancia: `const miInventario = new GestorInventario();`.
-   Agrega al menos 3 productos diferentes usando `miInventario.agregarProducto(...)` con datos de prueba.
-   Llama a `miInventario.listarProductos()` para verlos.
-   Busca un producto por ID: `const productoEncontrado = miInventario.buscarProductoPorId(2); console.log('Producto encontrado:', productoEncontrado);`.
-   Actualiza el stock de un producto: `miInventario.actualizarStock(1, 5, 'salida');`.
-   Vuelve a listar los productos para ver el cambio en el stock.

## Alternativas o Mejoras (Opcional):

-   **Búsqueda más flexible:** Podrías crear un método `buscarProductosPorNombre(termino: string): Producto[]` que devuelva todos los productos cuyo nombre contenga el término de búsqueda (insensible a mayúsculas/minúsculas).
    -   *Pista: `producto.nombre.toLowerCase().includes(termino.toLowerCase())`.*
-   **Validación de datos:** Antes de agregar un producto, podrías validar que el precio y el stock inicial no sean negativos.
-   **Archivos separados:** Considera tener `enums.ts`, `interfaces.ts`, `gestorInventario.ts` y `index.ts` para una mejor organización a medida que el proyecto crece.
