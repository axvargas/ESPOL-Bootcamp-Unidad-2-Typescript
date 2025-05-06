## 🔗 ¿Qué son las **Unions** en TypeScript?

Una **union** te permite que una variable tenga **más de un tipo posible**.  
Usamos el símbolo `|` (barra vertical) para decir: "**puede ser esto o aquello**".

### 🧠 Ejemplo básico:

```ts
let respuesta: string | number;

respuesta = "Aprobado";  // ✅
respuesta = 10;          // ✅
respuesta = true;        // ❌ Error: no es string ni number
```

➡️ Aquí, `respuesta` puede ser **una cadena o un número**, pero nada más.


### 🎯 ¿Para qué sirven?

- Cuando una variable puede tener **dos formas válidas**.
- Cuando un parámetro puede aceptar diferentes tipos.
- Para evitar usar `any`, pero seguir siendo flexibles.


### 🧪 Ejemplo con funciones:

```ts
function imprimirId(id: number | string) {
  console.log(`Tu ID es: ${id}`);
}
```

Aquí `id` puede ser un número (`123`) o un texto (`"abc123"`), y TypeScript lo permite sin que pierdas seguridad de tipos.

Además también te permite relizar uniones con el vlaor de una variable
```ts
type UserRole = "guest" | "member" | "admin"

let userRole: UserRole = "member"
// userRole = "something else" ❌ Error porque solo puede contener los valores espécificados
```

### 🛑 ¡Ojo! No puedes usar propiedades específicas de un tipo sin verificarlo primero:

```ts
function imprimirId(id: number | string) {
  // console.log(id.toUpperCase()); ❌ Error si es número

  if (typeof id === "string") {
    console.log(id.toUpperCase()); // ✅ Solo si es string
  }
}
```
---

## 🔤 ¿Qué son los **Literal Types** en TypeScript?

Los **Literal Types** son tipos que permiten que una variable **solo tenga un valor exacto**.  
Es como decir: “Esta variable **solo puede ser exactamente esto**”.

### 🎯 Ejemplo:

```ts
let direccion: "izquierda" | "derecha";

direccion = "izquierda"; // ✅
direccion = "derecha";   // ✅
direccion = "arriba";    // ❌ Error: solo se permite "izquierda" o "derecha"
```

### 🔐 ¿Para qué sirven?

- Para **limitar los valores válidos** que se pueden usar.
- Hacen el código **más seguro** y **más claro**.
- Muy útiles para valores tipo opción, estado o categoría.

### 🛠️ Usados con funciones:

```ts
function moverJugador(direccion: "izquierda" | "derecha") {
  console.log(`Moviendo al jugador hacia: ${direccion}`);
}
```

Llamar a esta función con `"arriba"` daría error porque **solo se aceptan los valores definidos literalmente**.


### ✅ Comparado con un string normal:

```ts
let estado: string;
estado = "activo";     // ✅ pero también acepta "loquesea" ❌

let estadoSeguro: "activo" | "inactivo";
estadoSeguro = "activo";   // ✅
estadoSeguro = "pausa";    // ❌ Error
```

### 📌 En resumen:

> **Literal types** te permiten restringir variables a **valores exactos**.  
> Son como enums ligeros y muy útiles cuando tienes un conjunto de opciones específicas.

---

## 🔍 ¿Qué es el **Type Narrowing**?

**Type narrowing** (o "reducción de tipo") es cuando TypeScript **deduce el tipo exacto** de una variable a partir de un tipo más amplio, como una unión (`string | number`), basándose en las condiciones que tú escribes en el código.

En otras palabras:  
> TypeScript “reduce” las posibilidades de tipo según los chequeos que haces.

### 🎯 Ejemplo:

```ts
function imprimir(valor: string | number) {
  if (typeof valor === "string") {
    console.log(valor.toUpperCase()); // Aquí TypeScript sabe que es string
  } else {
    console.log(valor.toFixed(2));    // Aquí sabe que es number
  }
}
```

👆 Antes del `if`, `valor` puede ser un `string` o un `number`.  
Dentro de cada bloque, TypeScript **reduce el tipo** al correcto según el chequeo.


### 🧠 ¿Por qué es útil?

- Te permite usar métodos específicos de cada tipo sin errores.
- Hace tu código **más seguro y más claro**.
- Evita usar `any` y te obliga a pensar en los diferentes caminos posibles.

### 🧪 Resumen:

**Type narrowing** es una herramienta de TypeScript que **deduce y restringe el tipo** de una variable dentro de una condición.  
Te permite trabajar con más seguridad cuando una variable puede ser de varios tipos.

---

## ✅ ¿Qué significa ser *explícito* en programación (y en TypeScript)?

Ser **explícito** significa dejar claro:
- Qué tipo de datos esperas.
- Qué puede y no puede pasar.
- Cómo deben comportarse tus funciones y variables.

Es lo contrario a asumir o dejar que el lenguaje "adivine".


### 📌 Veamos tu función `getHamburguerDetail`:

```ts
function getHamburguerDetail(identifier: string | number) {
```

Ya desde la firma de la función estás siendo **explícito** al decir:  
> “Esta función acepta como parámetro solo un `string` o un `number`”.  

TypeScript ya va a lanzar errores si alguien intenta pasar, por ejemplo, `null`, `boolean` o un objeto.


### 🔍 Type narrowing explícito

```ts
if (typeof identifier === "string") {
  // buscar por nombre
} else if (typeof identifier === "number") {
  // buscar por id
} else {
  throw new TypeError("Parameter `identifier` must be either a string or a number")
}
```

Aunque en teoría ese `else` **nunca debería ejecutarse** (porque el tipo ya está restringido), lo incluyes por si acaso alguien cambia la firma más adelante o desactiva el chequeo de tipos. Esto es una práctica de **código defensivo** y también es **ser explícito sobre errores posibles**.


No asumes que siempre se va a encontrar un resultado. Dejas claro qué pasa en caso contrario: **se muestra un error y se corta la ejecución**.


### 🧠 ¿Por qué ser explícito es tan importante?

- Te ayuda a **detectar errores antes de que pasen**.
- Hace que tu código sea **más fácil de entender** por otros (y por ti en el futuro).
- En equipos grandes o proyectos React complejos, ayuda a mantener el orden.


### 🧪 En resumen:

**Ser explícito** es decirle a TypeScript (y a otros desarrolladores)  
qué tipo de datos estás esperando, y qué hacer en cada situación.  
Tu función `getHamburguerDetail` es un excelente ejemplo de eso.

---
