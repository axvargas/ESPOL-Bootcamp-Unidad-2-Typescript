## 👤 Empezamos con el tipo `User`

Supongamos que queremos guardar usuarios en una app:

```ts
type User = {
  id: string
  name: string
  email: string
  age: number
}
```

---

## 🟦 ¿Qué es un Utility Type?

Un Utility Type es una **herramienta de TypeScript** que nos permite **crear nuevos tipos basados en otros**, pero **cambiando algunas reglas**, como quitar campos, hacerlos opcionales, etc.

---

## 1️⃣ `Omit` — Quitar propiedades

### 🎯 ¿Para qué sirve?

Para crear nuevos objetos **sin algunas propiedades**.
Ejemplo: cuando creamos un nuevo usuario, el `id` lo genera el sistema, no el usuario.

### ✅ Usamos `Omit` en `addNewUser`

```ts
type NewUser = Omit<User, 'id'>
```

Con esto creamos un nuevo tipo donde `id` **ya no es obligatorio ni permitido**.

### 🧠 Ejemplo:

```ts
function addNewUser(user: NewUser): User {
  const newUser: User = {
    ...user,
    id: generateId() // Función para crear un ID único
  }
  users.push(newUser)
  return newUser
}
```

### 📌 Así se usa:

```ts
addNewUser({ name: "Carlos", email: "carlos@gmail.com", age: 25 })
// No necesitas pasar el id
```

---

## 2️⃣ `Partial` — Hacer propiedades opcionales

### 🎯 ¿Para qué sirve?

Cuando quieres **actualizar un usuario existente**, no siempre quieres cambiar todos sus datos, solo uno o dos.

`Partial` hace que **todas las propiedades del tipo sean opcionales**.

```ts
function updateUser(id: string, updates: Partial<User>): User | undefined {
  const user = users.find(u => u.id === id)
  if (!user) return

  const updatedUser = { ...user, ...updates }
  return updatedUser
}
```

### 📌 Así se usa:

```ts
updateUser("123", { email: "nuevo@email.com" }) // solo cambia el email
updateUser("123", { age: 30 }) // solo cambia la edad
```

---

## 🧠 En resumen:

| Utility Type | ¿Qué hace?                                                     | ¿Cuándo usarlo?                                                    |
| ------------ | -------------------------------------------------------------- | ------------------------------------------------------------------ |
| `Omit<T, K>` | Crea un nuevo tipo quitando una o más propiedades (`K`) de `T` | Para funciones como `addNew`, donde no se necesita enviar el `id`  |
| `Partial<T>` | Vuelve todas las propiedades opcionales                        | Para funciones como `update`, donde solo actualizas algunos campos |

---

## 📘 Código completo de ejemplo:

```ts
type User = {
  id: string
  name: string
  email: string
  age: number
}

let users: User[] = []

function generateId(): string {
  return crypto.randomUUID()
}

type NewUser = Omit<User, 'id'>

function addNewUser(user: NewUser): User {
  const newUser: User = { ...user, id: generateId() }
  users.push(newUser)
  return newUser
}

function updateUser(id: string, updates: Partial<User>): User | undefined {
  const user = users.find(u => u.id === id)
  if (!user) return

  const updatedUser = { ...user, ...updates }
  return updatedUser
}
```

---

## 🧠 ¿Qué son los Generics?

Los **Generics** permiten crear componentes (funciones, tipos, clases) que pueden trabajar con **cualquier tipo de dato**, manteniendo la seguridad de tipos.

En vez de forzar un tipo (como `string`, `number`, etc.), usamos una **variable de tipo** como `T`, que puede adaptarse según se necesite.

---

### 🎯 ¿Por qué usar Generics?

Imagina que quieres hacer una función que devuelva el mismo valor que recibe. Puedes escribirla para cada tipo:

```ts
function echoString(value: string): string {
  return value
}

function echoNumber(value: number): number {
  return value
}
```

Pero con **Generics**, puedes escribir una sola función:

```ts
function echo<T>(value: T): T {
  return value
}

const result1 = echo("hola")     // result1 es string
const result2 = echo(123)        // result2 es number
const result3 = echo(true)       // result3 es boolean
```

TypeScript **infiera automáticamente el tipo**.

---

### 📦 Ejemplo con arrays

```ts
function firstItem<T>(array: T[]): T {
  return array[0]
}

const firstNumber = firstItem([1, 2, 3])        // number
const firstName = firstItem(["Ana", "Luis"])   // string
```

---

### 🔧 Ejemplo con tipo genérico en un objeto

```ts
type ApiResponse<T> = {
  success: boolean
  data: T
}

const response1: ApiResponse<string> = {
  success: true,
  data: "Todo salió bien"
}

const response2: ApiResponse<number[]> = {
  success: true,
  data: [1, 2, 3, 4]
}
```

---

## ✅ Desafío

> **Desafío:**
> Crea una función `wrapInArray` que reciba cualquier valor y lo devuelva dentro de un array.
> Usa generics para que TypeScript sepa qué tipo tiene el contenido del array.

---

