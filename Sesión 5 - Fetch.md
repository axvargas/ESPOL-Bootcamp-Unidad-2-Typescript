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

