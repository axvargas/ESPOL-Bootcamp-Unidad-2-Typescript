## 🧠 ¿Qué es una API?

**API** significa **Application Programming Interface**, o en español, **Interfaz de Programación de Aplicaciones**.

### 🎯 En palabras simples:

Una API es como un **mesero en un restaurante**:

* Tú (el cliente) haces un **pedido** (request)
* El mesero (API) lleva tu pedido a la cocina (servidor)
* La cocina prepara la comida (los datos)
* El mesero te trae la comida de vuelta (respuesta)

---

## 🌐 ¿Qué es una API web?

Una **API web** es una **forma de comunicarte con un servidor usando Internet**, normalmente desde tu app, sitio web o programa.

Por ejemplo, una API te permite:

* Obtener una lista de usuarios
* Crear una nueva cuenta
* Editar un producto
* Eliminar un comentario

---

## 📦 ¿Qué tipo de peticiones (requests) se pueden hacer?

Las APIs REST usan **métodos HTTP**, como:

| Método   | ¿Qué hace?                         | Ejemplo práctico                   |
| -------- | ---------------------------------- | ---------------------------------- |
| `GET`    | Leer datos                         | Ver lista de usuarios              |
| `POST`   | Crear nuevos datos                 | Registrar un nuevo usuario         |
| `PUT`    | Reemplazar datos existentes        | Editar todo un perfil de usuario   |
| `PATCH`  | Editar solo una parte de los datos | Cambiar solo el nombre del usuario |
| `DELETE` | Eliminar datos                     | Borrar un comentario               |

---
## 🧠 Resumen

* Una **API** es una forma de comunicarse con otro sistema.
* Las APIs web usan **peticiones HTTP** (`GET`, `POST`, `PUT`, `DELETE`, etc.).
* Sirven para **leer, crear, actualizar o borrar datos** desde tu aplicación.
* Se usan todo el tiempo en **React, TypeScript, JavaScript, móviles, etc.**

---

Observemos el siguiente código:
```ts
const API_URL = "https://6823c58065ba05803397d6df.mockapi.io/api/v1/users"

export async function getUsers(): Promise<Result<User[]>> {
  try {
    const res = await fetch(API_URL)
    if (!res.ok) throw new Error("Error al obtener los usuarios")
    const data = await res.json()
    return { success: true, data }
  } catch (error) {
    return { success: false, error: (error as Error).message }
  }
}

```

## 🧠 ¿Qué es `fetch`?

**`fetch` es una función que usamos para hacer peticiones a otras computadoras en internet.** Por ejemplo, cuando queremos:

* Obtener datos de una base de datos en la nube.
* Enviar un nuevo usuario.
* Actualizar información.
* Borrar datos.

---

### 📦 Imagina que:

Tu aplicación es como un niño que quiere saber algo. Entonces ese niño le manda una **carta (fetch)** a un servidor, preguntando:

> “¡Hola! ¿Me puedes mandar la lista de usuarios, por favor?”

Esa carta viaja por internet hasta llegar al servidor (MockAPI.io por ejemplo), y luego el servidor responde con otra carta que tiene los datos adentro. Esa respuesta puede tardar unos segundos en llegar.

---

## ⏳ ¿Por qué necesitamos `Promise`?

Porque **las cosas que tardan** en completarse **no dan una respuesta de inmediato**.

En la vida real:

* Si pides una pizza, no la tienes al instante.
* Si haces una pregunta a alguien que está lejos, tienes que esperar su respuesta.

En programación:

* Cuando usamos `fetch`, **no sabemos cuánto tardará la respuesta** del servidor.
* Por eso, **`fetch` devuelve una `Promise`**.

---

## ✅ ¿Qué es una `Promise`?

Una `Promise` es como una **promesa en la vida real**.

> "Te prometo que te voy a dar los datos cuando lleguen."

Hay 3 posibles estados:

1. **Pending**: la respuesta aún no ha llegado.
2. **Fulfilled**: todo salió bien, llegaron los datos.
3. **Rejected**: hubo un error (como que el servidor no responde).

---

## 🧠 ¿Por qué escribimos `Promise<Algo>`?

Cuando creamos funciones que hacen cosas que tardan, como `fetch`, debemos decirle a TypeScript:

> "Esta función **va a devolver una promesa**, y esa promesa **tendrá un valor de tipo `Algo` más adelante**."

Ejemplo:

```ts
async function getUsers(): Promise<User[]> {
  // ...
}
```

Esto significa:

> "Esta función va a tardar un poquito, y cuando termine, me va a dar un `array de usuarios` (`User[]`)."

---

## 🧃 ¿Y cómo esperamos esa promesa?

Usamos `await`. Es como decir:

> "Espera aquí hasta que llegue la respuesta."

Ejemplo:

```ts
const res = await fetch(API_URL)
```

Aquí le estamos diciendo a JavaScript:

> "Pide los datos con `fetch`, y **espera** hasta que lleguen antes de seguir."

---

## ✨ Resumen

| Concepto     | Explicación fácil                                                                |
| ------------ | -------------------------------------------------------------------------------- |
| `fetch`      | Función para hacer peticiones a servidores (como pedir datos por internet).      |
| `Promise`    | Una promesa que dice: "Cuando termine, te doy los datos o un error".             |
| `Promise<T>` | Tipo especial que dice: "La promesa traerá un valor de tipo `T` cuando termine". |
| `await`      | Le dice a JavaScript: "Espera tranquilamente a que la promesa termine".          |

---

## 🧠 ¿Qué es el tipo `Result`?

Es una forma ordenada de **decir si algo salió bien o salió mal** en nuestras funciones **sin usar `try/catch` fuera de la función** o sin lanzar errores que pueden romper la app.

---

### 🌈 Imaginemos dos cajas mágicas:

Cuando haces una función que usa `fetch`, puede pasar **una de dos cosas**:

1. **Todo salió bien** → y queremos devolver los datos.
2. **Algo salió mal** → y queremos decir qué pasó.

Pero en JavaScript normal, si algo sale mal, **lanza un error**, y si nadie lo atrapa… ¡💥 la app puede romperse!

---

## 🎁 ¿Qué hace `Result`?

**`Result` es una cajita mágica que siempre devuelve algo, pase lo que pase.**

### ✅ Si todo salió bien:

```ts
{ success: true, data: ... }
```

### ❌ Si hubo un error:

```ts
{ success: false, error: "Algo salió mal" }
```

Esto hace que **tu código sea más seguro y fácil de entender**.

---

## 🛠️ Ejemplo sencillo:

```ts
type Result<T> =
  | { success: true; data: T }
  | { success: false; error: string }
```

🔍 Significa:

* **`T` es el tipo de dato que quieres recibir si todo sale bien**.
* Pero si algo sale mal, simplemente recibes un `error`.

---

## 💡 Ventajas de usar `Result`:

| Ventaja ✨                         | ¿Por qué es útil?                                                               |
| --------------------------------- | ------------------------------------------------------------------------------- |
| No se rompen las funciones        | Nunca se lanza un error inesperado, se controla todo dentro de la función.      |
| Más fácil de leer                 | El resultado siempre tiene `success`, así sabes cómo actuar.                    |
| Más fácil de probar               | Puedes simular respuestas buenas y malas fácilmente.                            |
| Más claro para los alumnos nuevos | No tienen que aprender `try/catch` o excepciones desde el principio.            |
| Más predecible                    | Siempre sabes qué propiedades tendrá el resultado (`success`, `data`, `error`). |

---

## 🧪 ¿Cómo lo usamos?

```ts
const result = await getUsers()

if (result.success) {
  console.log("Usuarios recibidos:", result.data)
} else {
  console.error("Error:", result.error)
}
```

👶 Aquí estamos diciendo:

* “¿Salió todo bien?” → entonces usamos `result.data`
* “¿Falló algo?” → mostramos `result.error`

---

## 🚫 ¿Qué pasaría sin `Result`?

Si no usáramos `Result`, tendríamos que hacer esto:

```ts
try {
  const data = await getUsers()
  console.log(data)
} catch (error) {
  console.error("Algo falló:", error)
}
```

Esto **funciona**, pero:

* No es tan claro para principiantes.
* Puede ser fácil olvidar el `try/catch`.
* No tienes una estructura clara del resultado.

---

## 🎁 Conclusión

> Usar `Result` es como siempre recibir una **caja con instrucciones claras**:
>
> * Si fue un éxito: “¡Aquí están tus datos!”
> * Si fue un error: “Esto fue lo que pasó.”

Es **una forma clara, segura y ordenada** de trabajar con funciones que pueden fallar.

---

### 🔍 Código de getUser

```ts
export async function getUser(id: string): Promise<Result<User>> {
  try {
    const res = await fetch(`${API_URL}/${id}`)
    if (!res.ok) throw new Error("Usuario no encontrado")
    const data = await res.json()
    return { success: true, data }
  } catch (error) {
    return { success: false, error: (error as Error).message }
  }
}
```

---

## 🧠 Línea por línea explicada como para niños:

---

### `export async function getUser(id: string): Promise<Result<User>> {`

* **`export`**: Le dice a TypeScript que esta función se puede usar en otros archivos si la importamos.
* **`async function`**: Esta función es **asincrónica**, lo que significa que puede esperar cosas que tardan (como pedir datos por internet).
* **`getUser(id: string)`**: Es una función que se llama `getUser` y recibe un identificador (ID) como texto.
* **`: Promise<Result<User>>`**: La función **promete** (Promise) que más tarde te va a dar un `Result<User>`, es decir:

  * Si todo sale bien: `success: true, data: User`
  * Si algo va mal: `success: false, error: string`

---

### `try {`

* Aquí **intentamos ejecutar un bloque de código** que puede fallar.
* Si algo falla, vamos a **atrapar el error** con `catch`.

---

### `const res = await fetch(\`\${API\_URL}/\${id}\`)\`

* **`fetch(...)`**: Es como ir a buscar algo por internet (en este caso, un usuario).
* **`${API_URL}/${id}`**: Está armando una dirección web con el ID del usuario.

  * Si el API base es `https://ejemplo.com/api/users`, y el ID es `3`, entonces la dirección completa será `https://ejemplo.com/api/users/3`.
* **`await`**: Le dice a JavaScript: “Espera aquí hasta que tengas una respuesta”.

---

### `if (!res.ok) throw new Error("Usuario no encontrado")`

* **`res.ok`**: Pregunta si la respuesta fue exitosa (por ejemplo, código 200).
* **`!res.ok`**: Significa que algo salió mal (por ejemplo, 404).
* **`throw new Error(...)`**: Si hubo un problema, **lanzamos un error con un mensaje** personalizado: `"Usuario no encontrado"`.
* Lanzar un error **hace que el código salte al bloque `catch` de abajo**.

---

### `const data = await res.json()`

* **`res.json()`**: Convierte la respuesta en un objeto de JavaScript que podemos usar.
* **`await`**: De nuevo, espera hasta que esté listo.
* Al final, `data` será el usuario completo que vino de la API (por ejemplo: `{ id: "1", name: "Juan", age: 25 }`).

---

### `return { success: true, data }`

* Devuelve un **objeto del tipo `Result`** que dice que todo salió bien (`success: true`) y devuelve los datos del usuario.

---

### `} catch (error) {`

* Si en cualquier parte del `try` hubo un problema (por ejemplo, no hubo internet o el usuario no existía), **el código llega aquí**.

---

### `return { success: false, error: (error as Error).message }`

* Aquí devolvemos un `Result` de error.
* **`(error as Error).message`**: Estamos diciendo que el error tiene forma de `Error` para que podamos sacar su mensaje (por ejemplo, `"Usuario no encontrado"`).
* Devolvemos `{ success: false, error: "mensaje de error" }`.

---

## 🎁 Resultado final

Esta función nos garantiza **dos cosas importantes**:

1. Siempre recibiremos un objeto que dice si todo fue bien o mal.
2. Nunca se rompe la aplicación con errores inesperados.

---

## 🚀 ¿Qué podemos hacer con esta función?

```ts
const result = await getUser("3")

if (result.success) {
  console.log("El usuario es:", result.data)
} else {
  console.log("Ocurrió un error:", result.error)
}
```

---
### 🔍 Código de createUser

```ts
export async function createUser(input: CreateUser): Promise<Result<User>> {
  try {
    const res = await fetch(API_URL, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(input),
    })

    const data = await res.json()
    const result = userSchema.safeParse(data)
    if (!result.success) throw new Error("Respuesta inválida del servidor")

    return { success: true, data: result.data }
  } catch (error) {
    return { success: false, error: (error as Error).message }
  }
}
```
---

### 🧠 Fragmento del código:

```ts
const res = await fetch(API_URL, {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify(input),
})

const data = await res.json()
const result = userSchema.safeParse(data)
```

---

## 🔍 1. `fetch(...)`

```ts
const res = await fetch(API_URL, {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify(input),
})
```

### ¿Qué es `fetch`?

* Es una función que usamos para **hacer peticiones por internet**.
* En este caso, le estamos **enviando datos al servidor** para crear un nuevo usuario.

### ¿Qué significa cada parte?

* **`API_URL`**: Es la dirección a donde queremos mandar los datos (una URL como `https://mockapi.io/users`).
* **`method: "POST"`**: Le decimos al servidor que queremos **crear algo nuevo**.
* **`headers`**: Le decimos al servidor que **le estamos enviando JSON** (un formato de texto).
* **`body: JSON.stringify(input)`**: Convertimos el objeto `input` (por ejemplo: `{ name: "Juan", age: 25 }`) en una **cadena de texto JSON** para enviarla correctamente.

### ¿Y el `await`?

* Esperamos a que el servidor responda.
* El resultado se guarda en `res`.

---

## 🔍 2. `res.json()`

```ts
const data = await res.json()
```

### ¿Qué hace `res.json()`?

* El servidor devuelve una respuesta como texto.
* **`.json()` convierte ese texto en un objeto de JavaScript**.
* Por ejemplo: `"{\"id\": \"1\", \"name\": \"Juan\"}"` → `{ id: "1", name: "Juan" }`.

### ¿Por qué usamos `await`?

* Porque convertir la respuesta también **toma un poco de tiempo**.
* Le pedimos a JavaScript que **espere** a que termine.

---

## 🔍 3. `userSchema.safeParse(data)`

```ts
const result = userSchema.safeParse(data)
```

### ¿Qué es `safeParse`?

* Es una función que **valida** si los datos recibidos del servidor **siguen las reglas** del `userSchema`.
* Por ejemplo, si el usuario tiene:

  * un `id` que sea texto,
  * un `name` que no esté vacío,
  * una `age` que sea un número positivo,
  * un `email` válido,
  * y un `status` que sea `"active"` o `"inactive"`.

### ¿Por qué es importante?

* Porque aunque el servidor respondió, **queremos estar seguros de que los datos son correctos** y no están mal formados.
* `safeParse` **no lanza errores**, solo devuelve un objeto con:

  * `success: true` y `data` si los datos están bien.
  * `success: false` y `error` si los datos son incorrectos.

---

## ✨ Resumen

| Línea            | ¿Qué hace?                                        | ¿Para qué sirve?                 |
| ---------------- | ------------------------------------------------- | -------------------------------- |
| `fetch(...)`     | Manda los datos al servidor                       | Para crear el nuevo usuario      |
| `res.json()`     | Convierte la respuesta en objeto                  | Para poder leerla en JavaScript  |
| `safeParse(...)` | Verifica que los datos tengan el formato correcto | Para evitar errores más adelante |

---


Código:
```ts
import { User, CreateUser, UpdateUser, userSchema, createUserSchema } from "../types/userType"
import { Result } from "../types/resultType"

const API_URL = "https://6823c58065ba05803397d6df.mockapi.io/api/v1/users"

export async function getUsers(): Promise<Result<User[]>> {
  try {
    const res = await fetch(API_URL)
    if (!res.ok) throw new Error("Error al obtener los usuarios")
    const data = await res.json()
    return { success: true, data }
  } catch (error) {
    return { success: false, error: (error as Error).message }
  }
}

export async function getUser(id: string): Promise<Result<User>> {
  try {
    const res = await fetch(`${API_URL}/${id}`)
    if (!res.ok) throw new Error("Usuario no encontrado")
    const data = await res.json()
    return { success: true, data }
  } catch (error) {
    return { success: false, error: (error as Error).message }
  }
}

export async function createUser(input: CreateUser): Promise<Result<User>> {
  try {
    const res = await fetch(API_URL, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(input),
    })

    const data = await res.json()
    const result = userSchema.safeParse(data)
    if (!result.success) throw new Error("Respuesta inválida del servidor")

    return { success: true, data: result.data }
  } catch (error) {
    return { success: false, error: (error as Error).message }
  }
}

export async function updateUser(id: string, updates: UpdateUser): Promise<Result<User>> {
  try {
    const res = await fetch(`${API_URL}/${id}`, {
      method: "PUT",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(updates)
    })

    const data = await res.json()
    const result = userSchema.safeParse(data)
    if (!result.success) throw new Error("Respuesta inválida del servidor")

    return { success: true, data: result.data }
  } catch (error) {
    return { success: false, error: (error as Error).message }
  }
}

export async function deleteUser(id: string): Promise<Result<null>> {
  try {
    const res = await fetch(`${API_URL}/${id}`, { method: "DELETE" })
    if (!res.ok) throw new Error("No se pudo eliminar el usuario")
    return { success: true, data: null }
  } catch (error) {
    return { success: false, error: (error as Error).message }
  }
}

```
