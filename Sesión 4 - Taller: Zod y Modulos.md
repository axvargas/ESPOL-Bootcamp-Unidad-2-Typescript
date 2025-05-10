## 🎬 Pasos para inicializar un proyecto de TypeScript

Sí, **lo primero** que debes hacer es inicializar tu proyecto con:

```bash
npm init -y
```

Esto crea un archivo llamado `package.json`, que guarda la información de tu proyecto: nombre, dependencias, scripts, etc.

---

### 📦 Paso 2: Instalar TypeScript como dependencia de desarrollo

Debes instalar TypeScript como una **dependencia de desarrollo**, no como una dependencia de producción.

Lo correcto es:

```bash
npm install --save-dev typescript
```

Esto se hace porque TypeScript **no es parte de tu aplicación final**, solo lo usas mientras desarrollas, para compilar tu código a JavaScript. Luego, lo que corre realmente es el JavaScript compilado, no el TypeScript.

---

### 🤓 ¿Qué diferencia hay entre `--save-dev` y `--save`?

| Comando         | ¿Para qué sirve?                                                                        |
| --------------- | --------------------------------------------------------------------------------------- |
| `--save` o nada | Instala algo que **tu app necesita para funcionar** en producción. Ej: React, Zod, etc. |
| `--save-dev`    | Instala algo que solo usas mientras **desarrollas**. Ej: TypeScript, ESLint, etc.       |

---

### 📘 Paso 3: Crear archivo de configuración

Después de instalar TypeScript, puedes crear un archivo de configuración con:

```bash
npx tsc --init
```

Esto genera un archivo `tsconfig.json`, donde puedes configurar cosas como:

* La carpeta donde guardar los archivos `.js` generados
* Qué versiones de JavaScript usar
* Qué reglas aplicar, etc.

---

### ✅ Resumen Final

1. Sí, debes usar `npm init -y` al comenzar.
2. Instala TypeScript con:

   ```bash
   npm install --save-dev typescript
   ```
3. Genera el archivo de configuración con:

   ```bash
   npx tsc --init
   ```
---
Dentro de este archivo lo mejor siempre es descomentar la linea quie dice `outDir` y agregarle como valor `"dist"` para que todos nuestros archivos compilados se guarden en esa carpeta

## 🧠 ¿Qué es Zod?

Zod es una **librería de validación de datos**. Nos ayuda a asegurarnos de que los datos que estamos usando tienen la **forma y el tipo correcto**. Si alguien pone un número donde debería ir un texto, Zod lo detecta.

---

## 🛠️ Paso 1: Instalar Zod

En tu terminal escribe:

```bash
npm install zod
```

Esto agregará Zod a tu proyecto, y ya podrás usarlo desde tus archivos TypeScript.

---

## 👤 Paso 2: Crear un esquema de `User` con Zod

Imagina que un usuario debe tener:

* nombre (texto)
* edad (número entero, positivo, máximo 100)
* email (texto que sea un email válido)
* id (texto con formato UUID)
* estado (activo o inactivo)

### Código:

```ts
import { z } from "zod"

export const userSchema = z.object({
  id: z.string().uuid(),
  name: z.string(),
  age: z.number().int().positive().max(100),
  email: z.string().email(),
  status: z.enum(["active", "inactive"])
})

// Para obtener el tipo en TypeScript:
export type User = z.infer<typeof userSchema>
```

### ¿Qué hace cada línea?

* `z.string()` = solo permite texto.
* `z.number()` = solo permite números.
* `.int()` = debe ser un número entero.
* `.positive()` = debe ser positivo.
* `.max(100)` = no puede ser mayor a 100.
* `z.string().email()` = debe ser un email válido.
* `z.string().uuid()` = debe ser un texto con formato de UUID.
* `z.enum([...])` = solo acepta las opciones del arreglo.

---

### 🧠 ¿Qué es `export type User = z.infer<typeof userSchema>`?

Esta línea le está **diciendo a TypeScript**:

> "Por favor, crea un tipo llamado `User` a partir del esquema de Zod llamado `userSchema`".

---

### 🔍 Desglosemos cada parte:

```ts
export type User = z.infer<typeof userSchema>
```

#### 🧩 `userSchema`

Es el **esquema que tú creaste con Zod**, por ejemplo:

```ts
const userSchema = z.object({
  id: z.string().uuid(),
  name: z.string(),
  age: z.number().int().positive().max(100),
  email: z.string().email(),
  status: z.enum(["active", "inactive"]),
})
```

Este esquema dice cómo debe ser un objeto `User`.

---

#### 🔍 `typeof userSchema`

Esto le dice a TypeScript:

> "Quiero obtener el tipo de esa variable llamada `userSchema`".

---

#### 🧙 `z.infer<...>`

`z.infer` es una **función mágica de Zod** que le dice a TypeScript:

> "Mira ese esquema y crea un tipo automáticamente con esa forma".

Es como decirle:

> "Hazme el tipo que combine exactamente con ese esquema".

---

### ✅ Resultado

Después de hacer eso, ya puedes usar el tipo `User` como cualquier otro tipo en TypeScript:

```ts
const exampleUser: User = {
  id: "123e4567-e89b-12d3-a456-426614174000",
  name: "Juan",
  age: 30,
  email: "juan@example.com",
  status: "active"
}
```

Y TypeScript **sabrá si te equivocaste** con algún dato gracias a ese tipo `User`.

---

### ¿Por qué es útil?

✅ No tienes que escribir el tipo a mano.
✅ Siempre coincidirá con tu esquema Zod.
✅ Si cambias el esquema, el tipo se actualiza solo.
✅ Menos errores, más seguridad.

---

## ❓ ¿Qué es `unknown`?

`unknown` es un tipo de dato en TypeScript que dice: “no sé qué tipo es esto”. Lo usamos cuando los datos **vienen de fuera** (como de un formulario o de una API), y **debemos validarlos primero** antes de usarlos.

---

## ✅ Validar datos con `safeParse`

Vamos a validar que los datos sean correctos con Zod usando `safeParse`.

### Crear una función `validateInputData`:

```ts
function validateInputData(data: unknown): User | undefined {
  const result = userSchema.safeParse(data)
  if (!result.success) {
    console.error("Datos inválidos:", result.error.format())
    return
  }
  return result.data
}
```

### ¿Qué hace cada parte?

* `safeParse(data)` = intenta validar los datos.
* Si falla, muestra los errores.
* Si todo está bien, nos da los datos ya verificados y seguros.

---

## 🌟 1. ¿Qué hace `validateInputData`?

Esta función:

1. **Recibe un dato cualquiera** (no sabemos si está bien o mal).
2. Usa `safeParse` para **validar ese dato** con las reglas del `userSchema`.
3. Si el dato está mal, **muestra los errores** y devuelve `undefined`.
4. Si el dato está bien, **devuelve ese dato** ya validado.

---

## 🔍 2. ¿Cómo se usa y prueba?

Primero necesitas tener tu esquema y tu función:

### ✅ Esquema básico:

```ts
import { z } from "zod"

const userSchema = z.object({
  id: z.string().uuid({ message: "El ID debe ser un UUID válido" }),
  name: z.string({ required_error: "El nombre es obligatorio" }),
  age: z.number({ invalid_type_error: "La edad debe ser un número" })
         .int("Debe ser un número entero")
         .positive("Debe ser mayor que 0")
         .max(100, "La edad máxima es 100"),
  email: z.string().email({ message: "Email inválido" }),
  status: z.enum(["active", "inactive"], { errorMap: () => ({ message: "Estado inválido" }) })
})
```

### 🧠 ¿Qué es esto?

* Cada campo tiene **reglas claras**.
* También tiene **mensajes personalizados de error**, como "La edad máxima es 100".

---

### 🧪 Tu función de validación:

```ts
export type User = z.infer<typeof userSchema>

function validateInputData(data: unknown): User | undefined {
  const result = userSchema.safeParse(data)
  if (!result.success) {
    console.error("Datos inválidos:", result.error.format())
    return
  }
  return result.data
}
```

---

## 🧪 3. Ejemplos para probar

### ❌ Caso incorrecto

```ts
const wrongData = {
  id: "abc123", // no es UUID
  name: "",     // vacío
  age: 150,     // muy grande
  email: "not-an-email", // inválido
  status: "pending"      // no existe en el enum
}

validateInputData(wrongData)
// Mostrará todos los errores con los mensajes personalizados
```

### ✅ Caso correcto

```ts
const validData = {
  id: "123e4567-e89b-12d3-a456-426614174000",
  name: "María",
  age: 30,
  email: "maria@example.com",
  status: "active"
}

const user = validateInputData(validData)
console.log(user) // Aquí ves los datos ya parseados correctamente
```

---

## 📚 Explicación

Imagínate que el `userSchema` es **una lista de requisitos** para entrar a un club.

* Si alguien viene con un nombre, email, y edad correcta, puede entrar.
* Si se equivocó en el nombre o la edad, le mostramos una **nota personalizada** diciéndole qué está mal.

`safeParse` es como el **Guardia**. Si encuentra errores, te dice cuáles y no deja pasar al usuario.

---

## 🛠️ Omit y Partial con Zod

### Crear `CreateUser` (omitimos el id porque lo genera la app):

```ts
import { z } from "zod"
import { v4 as uuidv4 } from "uuid"

export const createUserSchema = userSchema.omit({ id: true })
export type CreateUser = z.infer<typeof createUserSchema>
```

### Crear `UpdateUser` (todos los campos opcionales):

```ts
export const updateUserSchema = createUserSchema.partial().omit({ id: true })
export type UpdateUser = z.infer<typeof updateUserSchema>
```

---

## 🧪 Funciones de `createUser` y `updateUser`

Tenemos una lista que simula una base de datos:

```ts
let users: User[] = []

function validateUserData(data: unknown): CreateUser | undefined {
  const result = createUserSchema.safeParse(data)
  if (!result.success) {
    console.error("Datos inválidos:", result.error.format())
    return
  }
  return result.data
}

function createUser(data: CreateUser): User {
  const newUser = { ...data, id: uuidv4() }
  users.push(newUser)
  return newUser
}

function updateUser(id: string, updates: UpdateUser): User | undefined {
  const user = users.find(u => u.id === id)
  if (!user) return
  const updatedUser = { ...user, ...updates }
  // actualizamos el array
  users = users.map(u => u.id === id ? updatedUser : u)
  return updatedUser
}

const user = validateUserData({
  name: "Ana",
  age: 28,
  email: "ana@example.com",
  status: "active",
})

let newUser
if(user){
  newUser = createUser(user)
  console.log("Usuario creado:", newUser)
} else {
  console.error('The user data is not valid')
  throw new Error("The user data is not valid");
}

```
---

## 🗂️ Modularización de archivos

Imaginemos esta estructura:

```
/types
  userTypes.ts
  productTypes.ts
/controllers
  usersController.ts
  productsController.ts
index.ts
```

### 1. En `userTypes.ts`:

```ts
import { z } from "zod"

export const userSchema = z.object({...})
export const createUserSchema = ...
export const updateUserSchema = ...

export type User = z.infer<typeof userSchema>
export type CreateUser = z.infer<typeof createUserSchema>
export type UpdateUser = z.infer<typeof updateUserSchema>
```

### 2. En `usersController.ts`:

```ts
import { CreateUser, UpdateUser, User } from "../types/userTypes"
import { v4 as uuidv4 } from "uuid"

let users: User[] = []

export function createUser(data: CreateUser): User {
  const user = { ...data, id: uuidv4() }
  users.push(user)
  return user
}

export function updateUser(id: string, updates: UpdateUser): User | undefined {
  ...
}
```

### 3. En `index.ts`:

```ts
import { createUser, updateUser } from "./controllers/usersController"

const u1 = createUser({ name: "Ana", age: 22, email: "ana@test.com", status: "active" })
const u2 = updateUser(u1.id, { age: 23 })
console.log(u1, u2)
```
---

## 🎯 ¿Qué es modularización?

**Modularizar** significa **dividir tu código en pedacitos**, como si armaras un LEGO.

En lugar de tener **todo el código en un solo archivo**, lo separas en archivos pequeños:

* Uno para los **tipos** (`types.ts`)
* Uno para las **funciones** (`userController.ts`)
* Uno para correr el programa (`index.ts`)

Esto hace que sea más fácil de **leer, mantener y entender**.

---

## 🧱 ¿Qué significa `export`?

La palabra `export` significa:
👉 “Quiero compartir esta parte del código con otros archivos”.

Ejemplo:

```ts
// types.ts
export const userSchema = z.object({ ... })
export type User = z.infer<typeof userSchema>
```

Aquí decimos: "Voy a exportar el esquema y el tipo `User` para que otro archivo los pueda usar".

---

## 📥 ¿Qué significa `import`?

La palabra `import` significa:
👉 “Voy a traer algo de otro archivo”.

Ejemplo:

```ts
// userController.ts
import { userSchema, User } from "./types"
```

Esto le dice a TypeScript:
"Tráeme el esquema y el tipo `User` del archivo llamado `types.ts`".

---

## 🎒 ¿En qué ayuda modularizar?

1. **Organización**: Separas responsabilidades.

   * `types.ts` define los datos (como reglas del juego).
   * `userController.ts` tiene las funciones (como crear o editar usuarios).
   * `index.ts` es donde todo se une y corre (como el botón de "jugar").

2. **Reutilización**: Puedes usar los mismos tipos o funciones en diferentes partes del proyecto sin copiar y pegar.

3. **Escalabilidad**: Si tu aplicación crece, modularizar te permite trabajar más fácilmente con equipos o añadir más funciones.
---

## ⚙️ Compilar y probar

1. Asegúrate de tener `tsconfig.json`.
2. En la terminal:

```bash
npx tsc
node dist/index.js
```

Esto compilará y ejecutará tu proyecto.

---
