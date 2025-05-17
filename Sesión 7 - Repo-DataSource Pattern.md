# Patrones de diseño para el manejo de datos

Ahora podemos hacer uso de patrones de diseño que usan los conceptos aprendidos con aterioridad, para ordernear de manera profesional nuestros proyectos que involucran datos en APIs para ello vamos a hacer uso de 
- Datasource Pattern
- Repository Prattern
- Factory Pattern

Siguiendo la siguiente estructura:

![Repository +  DataSource + Factory Pattern drawio (3)](https://github.com/user-attachments/assets/1f8beb4d-54a8-419c-b7b2-12139b69761a)


## 🧱 1. Aplicación (Application Layer)

**¿Qué hace?**

* Es la **entrada principal** de tu aplicación.
* Llama a los **repositorios** para obtener o modificar datos.
* Coordina acciones entre distintas capas (por ejemplo, mostrar datos al usuario, guardar nuevos registros, etc).

> ✅ Piensa en esto como el **cerebro que organiza las acciones** y usa los demás componentes.

---

## 🏭 2. Repository Factory

**¿Qué hace?**

* Se encarga de **crear e instanciar los repositorios** con sus respectivas dependencias (como los data sources).
* Centraliza toda la lógica de construcción.

> ✅ Es el **constructor inteligente** que crea repositorios y resuelve sus dependencias por ti.

---

## 🧩 3. Repository Implementation (Implementa `Repository`)

**¿Qué hace?**

* Es una **clase concreta** que implementa la interfaz `Repository`.
* Contiene la lógica de negocio como `getAll()`, `create()`, `update()`, etc.
* Se comunica con el `DataSource`, pero **no sabe si viene de una API, base de datos o mock**.

> ✅ El repositorio es el **puente entre la lógica de negocio y los datos**, y te protege de los detalles de implementación.

---

## 🌐 4. DataSource

**¿Qué hace?**

* Se encarga de **acceder directamente a la fuente de datos**: puede ser una API REST, Firebase, una base de datos local, o un mock.
* Define funciones como `fetchUsers()`, `postUser()`, etc.

> ✅ Es el **encargado de hablar con el mundo externo** (API, archivos, etc).

---

## 🛠 5. API (o servicio externo)

**¿Qué hace?**

* Es un **servicio remoto** que maneja tus datos reales.
* Normalmente es un servidor REST al que accedes usando `fetch`, `axios`, etc.

> ✅ Es la **fuente de datos externa**, a la cual no tienes control directo (pero puedes consumirla).

---
Perfecto, vamos a explicar paso a paso los tres patrones de diseño: **Repository**, **DataSource** y **Factory**, usando un ejemplo real con MockAPI.io para una aplicación que maneja usuarios. Como tus alumnos son nuevos en estos conceptos, explicaré todo con palabras simples y ejemplos claros.

---

## 🧠 1. ¿Qué es cada patrón?

### 🌐 DataSource Pattern (Patrón de Fuente de Datos)

**¿Qué es?**
Es la clase que se encarga directamente de hablar con el "mundo exterior" (una API, una base de datos, un archivo, etc.).

**¿Para qué sirve?**
Para centralizar todo el código que se encarga de obtener datos desde fuera. Así, si mañana dejamos de usar MockAPI y usamos otra API, solo cambiamos el `DataSource`, no todo el código.

**Ejemplo real:**
Un archivo `UserApiDataSource.ts` que tiene las funciones que usan `fetch` para conectarse con MockAPI.io.

---

### 📦 Repository Pattern (Patrón de Repositorio)

**¿Qué es?**
Es una capa de la aplicación que se encarga de definir qué operaciones se pueden hacer con nuestros datos (crear, leer, actualizar, borrar, etc.).

**¿Para qué sirve?**
Para separar la lógica del negocio (por ejemplo, "crear un usuario") de la lógica de acceso a datos (por ejemplo, "usar fetch para pedir datos a una API").

**Ejemplo real:**
Una interfaz `UserRepository.ts` con funciones como `createUser`, `getUserById`, etc. La implementación (`UserRepositoryImpl.ts`) se conecta a un `DataSource` para obtener esos datos.

---

### 🏭 Factory Pattern (Patrón de Fábrica)

**¿Qué es?**
Es una función que crea y devuelve objetos listos para usar (como nuestros repositorios).

**¿Para qué sirve?**
Para centralizar la creación de objetos, especialmente cuando hay muchas dependencias (por ejemplo, un repositorio necesita un data source).

**Ejemplo real:**
`createRepositories.ts` contiene una función `createUserRepository()` que crea e inyecta un `UserApiDataSource` dentro de `UserRepositoryImpl`.

---

## 🗂 Estructura de carpetas y archivos

```
/types
  └── userType.ts         👤 Define el tipo y esquema Zod del usuario
  └── Result.ts           ✅ Define el tipo Result<T> para errores o datos

/datasources
  └── UserApiDataSource.ts 🌐 Implementa las llamadas reales con fetch

/repositories
  └── UserRepository.ts     📦 Interfaz con los métodos que usamos
  └── UserRepositoryImpl.ts 🔧 Implementación que usa el DataSource

/factories
  └── createRepositories.ts 🏭 Crea los repositorios con sus dependencias

index.ts                    🚀 Punto de entrada para probar el código
```

---

¡Perfecto! Comencemos explicando los primeros dos archivos esenciales que nos permiten trabajar con datos de manera segura y clara en nuestra aplicación.

---

## 🗂 Archivo `/types/userType.ts`

### ✨ ¿Qué contiene?

Define el **esquema de validación con Zod** y el **tipo `User`** que representa a un usuario.

### 📄 Código y explicación

```ts
// /types/userType.ts
import { z } from "zod"
import { Result } from "./Result"

// 1. Esquema base del usuario
export const userSchema = z.object({
  id: z.string().uuid(),
  name: z.string().min(1, "El nombre es obligatorio"),
  age: z.number().int().positive().max(100),
  email: z.string().email("Email inválido"),
  status: z.enum(["active", "inactive"]),
})

// 2. Esquema para creación (sin id)
export const createUserSchema = userSchema.omit({ id: true })

// 3. Esquema para actualización (todos opcionales excepto id)
export const updateUserSchema = createUserSchema.partial()

// 4. Tipos TypeScript derivados
export type User = z.infer<typeof userSchema>
export type CreateUser = z.infer<typeof createUserSchema>
export type UpdateUser = z.infer<typeof updateUserSchema>

// 5. Funciones de validación
export function validateCreateUser(input: unknown): Result<CreateUser> {
  const result = createUserSchema.safeParse(input)
  if (!result.success) {
    return { success: false, error: result.error.message }
  }
  return { success: true, data: result.data }
}

export function validateUpdateUser(input: unknown): Result<UpdateUser> {
  const result = updateUserSchema.safeParse(input)
  if (!result.success) {
    return { success: false, error: result.error.message }
  }
  return { success: true, data: result.data }
}

```

### 🧠 ¿Qué conceptos nuevos aparecen?

* `z.object()` define un esquema con reglas.
* `.min()`, `.max()`, `.email()` son validaciones.
* `z.enum()` valida que el valor esté dentro de una lista.
* `z.infer<typeof schema>` genera un tipo TypeScript automáticamente.
* `.omit()` elimina campos del esquema (como `id`).
* `.partial()` hace que todos los campos sean opcionales (para actualizar).

---

## 🗂 Archivo `/types/Result.ts`

### ✨ ¿Qué contiene?

Define un tipo genérico `Result<T>` para manejar resultados exitosos o con error.

### 📄 Código y explicación

```ts
// /types/Result.ts

// Si la operación fue exitosa, `success: true` y contiene los datos
// Si falló, `success: false` y contiene el mensaje de error
export type Result<T> =
  | { success: true; data: T }
  | { success: false; error: string }
```

### 🧠 ¿Por qué lo usamos?

Nos permite **manejar errores de forma segura y clara**.
En vez de lanzar errores con `throw`, devolvemos un `Result` para que quien use la función pueda decidir qué hacer.
---

---

## 🗂 Archivo `/datasources/UserApiDataSource.ts`

### 📌 ¿Qué es un "Data Source"?

Un **Data Source** es la capa que **sabe cómo acceder a los datos**, ya sea desde una API, base de datos, archivo, etc.

En nuestro caso, se encarga de hacer llamadas a **MockAPI.io** usando `fetch`.

---

### 📄 Código con explicación paso a paso

```ts
// /datasources/UserApiDataSource.ts
import { CreateUser, UpdateUser, User, userSchema } from "../types/userType"
import { Result } from "../types/Result"

const API_URL = "https://6823c58065ba05803397d6df.mockapi.io/api/v1/users"

export class UserApiDataSource {
  async getUsers(): Promise<Result<User[]>> {
    try {
      const res = await fetch(API_URL)
      if (!res.ok) throw new Error("Error al obtener los usuarios")
      const data = await res.json()
      return { success: true, data }
    } catch (error) {
      return { success: false, error: (error as Error).message }
    }
  }

  async getUser(id: string): Promise<Result<User>> {
    try {
      const res = await fetch(`${API_URL}/${id}`)
      if (!res.ok) throw new Error("Usuario no encontrado")
      const data = await res.json()
      return { success: true, data }
    } catch (error) {
      return { success: false, error: (error as Error).message }
    }
  }

  async createUser(input: CreateUser): Promise<Result<User>> {
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

  async updateUser(id: string, input: UpdateUser): Promise<Result<User>> {
    try {
      const res = await fetch(`${API_URL}/${id}`, {
        method: "PUT",
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

  async deleteUser(id: string): Promise<Result<null>> {
    try {
      const res = await fetch(`${API_URL}/${id}`, {
        method: "DELETE",
      })

      if (!res.ok) throw new Error("No se pudo eliminar el usuario")
      return { success: true, data: null }
    } catch (error) {
      return { success: false, error: (error as Error).message }
    }
  }
}
```

---

### 🧠 ¿Qué conceptos nuevos vemos aquí?

* `class UserApiDataSource`: usamos una **clase** para agrupar funciones relacionadas con la API.
* `fetch`: es la función que usamos para hacer **peticiones HTTP**.
* `async/await`: permite trabajar con promesas de forma **más clara y secuencial**.
* `JSON.stringify(input)`: convierte el objeto en texto para enviarlo como cuerpo (`body`) de la petición.
* `.safeParse()`: valida la respuesta de la API antes de usarla.
* `Result<T>`: encapsula el éxito o error de cada función.

---

### 🧪 ¿Qué hace cada método?

| Método                 | Qué hace                                       |
| ---------------------- | ---------------------------------------------- |
| `getUsers()`           | Obtiene la lista de usuarios.                  |
| `getUser(id)`          | Busca un solo usuario por su `id`.             |
| `createUser(data)`     | Crea un nuevo usuario con los datos recibidos. |
| `updateUser(id, data)` | Actualiza los datos del usuario con ese `id`.  |
| `deleteUser(id)`       | Elimina el usuario por `id`.                   |

---

## 🗂 Archivo `/repositories/UserRepository.ts`

### 📌 ¿Qué es el patrón Repository?

El **Repository Pattern** define una **interfaz (contrato)** que nos dice qué operaciones podemos hacer sobre los datos (por ejemplo: crear, obtener, actualizar, eliminar), **sin importar de dónde vienen esos datos** (una API, una base de datos, un archivo, etc).

Esto nos da **flexibilidad y separación de responsabilidades**:

* Tu aplicación solo conoce el "qué se puede hacer".
* El "cómo se hace" lo decide la clase que **implementa** esa interfaz.

---

### 📄 Código del repositorio

```ts
// /repositories/UserRepository.ts
import { CreateUser, UpdateUser, User } from "../types/userType"
import { Result } from "../types/Result"

// Esta interfaz define lo que un repositorio de usuarios debe poder hacer
export interface UserRepository {
  getUsers(): Promise<Result<User[]>>
  getUser(id: string): Promise<Result<User>>
  createUser(input: CreateUser): Promise<Result<User>>
  updateUser(id: string, input: UpdateUser): Promise<Result<User>>
  deleteUser(id: string): Promise<Result<null>>
}
```

---

### 🧠 ¿Qué conceptos aparecen aquí?

* `interface UserRepository`: Es una **interfaz**. Piensa en ella como un contrato que dice: "cualquier clase que quiera ser un UserRepository debe tener estos métodos".
* `Promise<Result<T>>`: Cada método devuelve una promesa con un objeto `Result`, que puede ser éxito o error.
* Usamos los tipos ya definidos: `User`, `CreateUser`, `UpdateUser`.

---

### 🤔 ¿Por qué es útil?

Supongamos que un día cambias de MockAPI a Firebase. Solo necesitarías hacer una nueva implementación de `UserRepository`, sin cambiar nada del resto de la app. 🎉

---

| **Ventajas del Repository Pattern**                       |
| --------------------------------------------------------- |
| 🔁 Intercambiar fácilmente la fuente de datos.            |
| 💡 Facilita los tests (puedes usar un repositorio falso). |
| 📦 Encapsula la lógica de acceso a datos.                 |
| 🤝 Separa el *qué hago* del *cómo lo hago*.               |

---

## 🧩 Archivo `/repositories/UserRepositoryImpl.ts`

Aquí aplicamos el **Repository Pattern** completamente, haciendo que esta clase **"conecte" tu app con una fuente de datos (la API)**.

---

### 📄 Código de ejemplo:

```ts
// /repositories/UserRepositoryImpl.ts
import { UserRepository } from "./UserRepository"
import { UserApiDataSource } from "../datasources/UserApiDataSource"
import { CreateUser, UpdateUser, User } from "../types/userType"
import { Result } from "../types/Result"

// Esta clase IMPLEMENTA el contrato UserRepository
export class UserRepositoryImpl implements UserRepository {
  constructor(private readonly dataSource: UserApiDataSource) {}

  async getUsers(): Promise<Result<User[]>> {
    return this.dataSource.getUsers()
  }

  async getUser(id: string): Promise<Result<User>> {
    return this.dataSource.getUser(id)
  }

  async createUser(input: CreateUser): Promise<Result<User>> {
    return this.dataSource.createUser(input)
  }

  async updateUser(id: string, input: UpdateUser): Promise<Result<User>> {
    return this.dataSource.updateUser(id, input)
  }

  async deleteUser(id: string): Promise<Result<null>> {
    return this.dataSource.deleteUser(id)
  }
}
```

---

### 🧠 ¿Qué está pasando línea a línea?

* `implements UserRepository`: Esta clase promete tener **todos los métodos** definidos por la interfaz.
* `constructor(private readonly dataSource: UserApiDataSource)`: Se le inyecta una clase que contiene la lógica real para hablar con la API.
* Cada método como `getUsers` o `createUser` simplemente **reenvía la llamada a `dataSource`**.

---

### 🧱 ¿Por qué hacemos esto?

* Así **separamos responsabilidades**:

  * `UserApiDataSource`: Sabe cómo hablar con la API.
  * `UserRepositoryImpl`: Sabe **qué operaciones hay que hacer** con los usuarios, pero **sin preocuparse del "cómo" se hace**.
* Si mañana cambias de MockAPI a Firebase, solo reemplazas el `UserApiDataSource`.

---

| ✅ Ventajas                                                               |
| ------------------------------------------------------------------------ |
| Puedes cambiar la fuente de datos sin cambiar el repositorio.            |
| Puedes simular un `FakeUserDataSource` para pruebas.                     |
| Tu app depende del contrato (`UserRepository`), no de la implementación. |

---

¡Vamos allá! Ahora toca implementar el **Factory Pattern**. Este patrón se encarga de **crear y configurar** los objetos que necesita tu aplicación, como los repositorios o los data sources.

---

## 🏭 Archivo `/factories/createRepositories.ts`

Este archivo es responsable de:

1. Crear el `UserApiDataSource` (que habla con la API).
2. Crear el `UserRepositoryImpl` usando ese data source.
3. Exportar el repositorio listo para ser usado.

---

### 📄 Código:

```ts
// /factories/createRepositories.ts
import { UserApiDataSource } from "../datasources/UserApiDataSource"
import { UserRepositoryImpl } from "../repositories/UserRepositoryImpl"

// 🔧 Fábrica que crea todos los repositorios
export function createRepositories() {
  const userApiDataSource = new UserApiDataSource()
  const userRepository = new UserRepositoryImpl(userApiDataSource)

  return {
    userRepository,
  }
}
```

---

### 🧠 ¿Qué hace este código?

* Se **centraliza la creación** de objetos importantes como `dataSource` y `repository`.
* Si en el futuro `UserApiDataSource` necesita configuraciones especiales (ej: un token), puedes hacerlo **solo aquí**.
* El resultado es un objeto `{ userRepository }` que puedes usar en tu aplicación.

---

### 🧱 ¿Por qué usar Factory?

| Ventaja                  | Explicación                                                                                           |
| ------------------------ | ----------------------------------------------------------------------------------------------------- |
| Centralización           | Todo lo necesario para crear repositorios está en un solo lugar.                                      |
| Escalabilidad            | Si mañana agregas más repositorios, los defines aquí también.                                         |
| Configuración más limpia | Si un `dataSource` necesita parámetros o autenticación, puedes configurarlo aquí sin ensuciar tu app. |

---

## 🧩 Archivo `index.ts`

Este archivo será el punto de entrada de tu aplicación. Aquí:

1. Usaremos la **Factory** para obtener el `userRepository`.
2. Ejecutaremos operaciones como:

   * `createUser`
   * `getUser`
   * `updateUser`
   * `deleteUser`
   * `getUsers`
3. Mostraremos resultados y errores en consola.

---

### 📄 Código:

```ts
// index.ts
import { createRepositories } from "./factories/createRepositories"
import { validateCreateUser, validateUpdateUser } from "./types/userType"

async function main() {
  const { userRepository } = createRepositories()

  // 📥 Crear un usuario
  const input = {
    name: "Ana",
    age: 25,
    email: "ana@example.com",
    status: "active",
  }

  const validatedCreate = validateCreateUser(input)
  if (!validatedCreate.success) {
    console.error("Error de validación al crear:", validatedCreate.error)
    return
  }

  const created = await userRepository.createUser(validatedCreate.data)
  if (!created.success) return console.error("Error al crear:", created.error)
  console.log("✅ Usuario creado:", created.data)

  // 🔍 Obtener un usuario por ID
  const userId = created.data.id
  const fetched = await userRepository.getUser(userId)
  if (!fetched.success) return console.error("Error al obtener:", fetched.error)
  console.log("👤 Usuario obtenido:", fetched.data)

  // ✏️ Editar usuario
  const updatedData = { name: "Ana Actualizada", age: 26 }
  const validatedUpdate = validateUpdateUser(updatedData)
  if (!validatedUpdate.success) {
    console.error("Error de validación al actualizar:", validatedUpdate.error)
    return
  }

  const updated = await userRepository.updateUser(userId, validatedUpdate.data)
  if (!updated.success) return console.error("Error al actualizar:", updated.error)
  console.log("🔄 Usuario actualizado:", updated.data)

  // 📋 Obtener todos los usuarios
  const all = await userRepository.getUsers()
  if (!all.success) return console.error("Error al obtener lista:", all.error)
  console.log("📚 Todos los usuarios:", all.data)

  // 🗑️ Eliminar usuario
  const deleted = await userRepository.deleteUser(userId)
  if (!deleted.success) return console.error("Error al eliminar:", deleted.error)
  console.log("🗑️ Usuario eliminado:", deleted.data)
}

main()
```

---

### 🧠 ¿Qué está pasando aquí?

| Sección                | Explicación                                                     |
| ---------------------- | --------------------------------------------------------------- |
| `createRepositories()` | Usamos el patrón Factory para obtener un repositorio funcional. |
| `validateCreateUser()` | Validamos el input antes de hacer el POST.                      |
| `createUser()`         | Creamos un usuario en la API con `POST`.                        |
| `getUser()`            | Obtenemos el usuario por ID desde la API.                       |
| `updateUser()`         | Editamos datos usando `PUT`.                                    |
| `getUsers()`           | Traemos todos los usuarios (`GET`).                             |
| `deleteUser()`         | Borramos un usuario (`DELETE`).                                 |

---

