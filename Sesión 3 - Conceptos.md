¡Claro! Aquí tienes una explicación clara, paso a paso y con ejemplos **muy fáciles de seguir** usando un objeto de tipo `User`, para que tus alumnos entiendan bien cómo y **cuándo usar los Utility Types** `Omit` y `Partial`.

---

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

