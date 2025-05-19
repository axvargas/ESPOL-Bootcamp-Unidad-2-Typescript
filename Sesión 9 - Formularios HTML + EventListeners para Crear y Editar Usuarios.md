# 📚 Formularios HTML + EventListeners para Crear y Editar Usuarios

---

## 🧱 Parte 1: Entendiendo Formularios HTML

### 🧾 ¿Qué es un formulario?

Un formulario HTML (`<form>`) permite **recopilar datos** del usuario.

#### 📄 index.html
```html
<!DOCTYPE html>
<html>

<head>
  <meta charset="UTF-8">
  <title>CRUD Usuarios</title>
  <link rel="stylesheet" type="text/css" href="https://cdn.datatables.net/2.3.1/css/dataTables.dataTables.min.css">
</head>

<body>
  <h2>Crear Usuario</h2>
  <form id="createUserForm">
    <input type="text" name="name" placeholder="Nombre" required />
    <input type="number" name="age" placeholder="Edad" required />
    <input type="email" name="email" placeholder="Email" required />
    <select name="status">
      <option value="active">Activo</option>
      <option value="inactive">Inactivo</option>
    </select>
    <button type="submit">Crear</button>
  </form>

  <h2>Editar Usuario</h2>
  <form id="editUserForm" style="display: none;">
    <input type="hidden" name="id" />
    <input type="text" name="name" placeholder="Nombre" required />
    <input type="number" name="age" placeholder="Edad" required />
    <input type="email" name="email" placeholder="Email" required />
    <select name="status">
      <option value="active">Activo</option>
      <option value="inactive">Inactivo</option>
    </select>
    <button type="submit">Actualizar</button>
  </form>

  <h1>Lista de Usuarios</h1>
  <table id="user-table">
  </table>
  <script type="module" src="/src/main.ts"></script>
</body>

</html>
```

---

## 🎯 ¿Qué pasa cuando se envía un formulario?

El navegador, por defecto, **recarga la página**.

**Para evitar eso:**

```ts
form.addEventListener("submit", (event) => {
  event.preventDefault() // ❌ Evita el reload
  // ✅ Aquí procesamos los datos
})
```
#### 📄 /src/views/userCreateForm.ts
```ts
import { createRepositories } from "../api/factory/createRepositories"
import type { CreateUser } from "../types/userType"
import { renderTable } from "./userTable"


const {userRepository} = createRepositories()
const createForm = document.getElementById("createUserForm") as HTMLFormElement

export async function setUserCreateFormEvents() {
  // Crear usuario
  createForm.addEventListener("submit", async (e) => {
    e.preventDefault()
    const formData = new FormData(createForm)
    const input = {
      name: formData.get("name"),
      age: Number(formData.get("age")),
      email: formData.get("email"),
      status: formData.get("status")
    } as CreateUser

    const result = await userRepository.createUser(input)
    if (result.success) {
      alert("Usuario creado")
      createForm.reset()
      renderTable()
    } else {
      alert(result.error)
    }
  })
}
```
---

## 🔹 ¿Qué hace `document.getElementById`?

Busca un elemento en el HTML con el **id** indicado:

```html
<form id="createUserForm">...</form>
```

Entonces esta línea:

```ts
document.getElementById("createUserForm")
```

devuelve ese formulario.

---

## 🔹 ¿Por qué se usa `as HTMLFormElement`?

Por defecto, TypeScript no sabe que ese elemento es un formulario. Solo sabe que es un `HTMLElement`.

Pero para usar funciones especiales de formularios como `.reset()` o para crear un `FormData`, necesitamos decirle a TypeScript:
📢 *“¡Esto es un formulario!”*

Por eso usamos:

```ts
as HTMLFormElement
```

✅ Así TypeScript sabe que puede usar:

* `createForm.reset()`
* `new FormData(createForm)`

---

## 🎓 Mini ejemplo:

```ts
const createForm = document.getElementById("createUserForm") as HTMLFormElement
```

Lo estamos "convirtiendo" (haciendo un *type assertion*) en un formulario para trabajar con él más fácilmente.

---

## 🧾 ¿Qué es `new FormData(createForm)`?

`FormData` es una clase que nos permite **leer todos los valores del formulario** de manera automática.

```ts
const formData = new FormData(createForm)
```

Esto:

* Toma el formulario
* Revisa todos los `input`, `select`, `textarea`, etc.
* Guarda sus valores

---

### 🎓 Ejemplo:

```html
<form id="createUserForm">
  <input type="text" name="name" value="Ana" />
  <input type="email" name="email" value="ana@email.com" />
</form>
```

```ts
const formData = new FormData(createForm)
const name = formData.get("name") as string
const email = formData.get("email") as string

console.log(name)  // "Ana"
console.log(email) // "ana@email.com"
```

> ⚠️ Nota: El método `.get()` devuelve un tipo `FormDataEntryValue | null`, por eso usamos `as string` si estamos seguros de que existe.

---

## 🔄 ¿Qué hace `createForm.reset()`?

Limpia todos los campos del formulario. Los deja como estaban cuando se cargó la página.

```ts
createForm.reset()
```

💡 Es útil cuando ya creamos el usuario y queremos dejar el formulario vacío para crear otro.

---

## ✅ Recapitulación en una tabla

| Código                             | ¿Qué hace?                                   |
| ---------------------------------- | -------------------------------------------- |
| `getElementById("createUserForm")` | Busca un elemento por su ID                  |
| `as HTMLFormElement`               | Le dice a TypeScript que es un `<form>`      |
| `new FormData(form)`               | Extrae los datos del formulario              |
| `formData.get("name")`             | Obtiene el valor del campo con `name="name"` |
| `form.reset()`                     | Limpia todos los campos del formulario       |

---

Ahora podremos hacer funcionar el formulario de edición. Para esto tendremos que agregar un eventListener para el botón de edición:
#### 📄 /src/views/userTable.ts
```ts
...
export async function setDatatableEvents(){
  tableElement.addEventListener("click", async (e) => {
    const target = e.target as HTMLElement
    const id = target.dataset.id

    if (!id) return
  
    if (target.classList.contains("edit-btn")) {
      const result = await userRepository.getUser(id)
      if (result.success) {
        const user = result.data
        editForm.style.display = "block"
        const idInput = editForm.elements.namedItem("id") as HTMLInputElement
        const nameInput = editForm.elements.namedItem("name") as HTMLInputElement
        const ageInput = editForm.elements.namedItem("age") as HTMLInputElement
        const emailInput = editForm.elements.namedItem("email") as HTMLInputElement
        const statusInput = editForm.elements.namedItem("status") as HTMLSelectElement
        idInput.value = user.id
        nameInput.value = user.name
        ageInput.value = String(user.age)
        emailInput.value = user.email
        statusInput.value = user.status
      }
    }
  
    if (target.classList.contains("delete-btn")) {
      if (confirm("¿Eliminar este usuario?")) {
        const result = await userRepository.deleteUser(id)
        if (result.success) {
          alert("Usuario eliminado")
          renderTable()
        } else {
          alert(result.error)
        }
      }
    }
  })
}
```
Perfecto, esta es una excelente sección para enseñar cómo acceder a los campos de un formulario **de forma segura y tipada** en TypeScript. Vamos a explicarlo paso a paso, con ejemplos y de forma simple para que tus alumnos lo entiendan desde cero.

---

### 📌 Código:

```ts
const idInput = editForm.elements.namedItem("id") as HTMLInputElement
const nameInput = editForm.elements.namedItem("name") as HTMLInputElement
const ageInput = editForm.elements.namedItem("age") as HTMLInputElement
const emailInput = editForm.elements.namedItem("email") as HTMLInputElement
const statusInput = editForm.elements.namedItem("status") as HTMLSelectElement
```

---

## 🔍 Desglose línea por línea

---

### 🔹 `editForm.elements.namedItem("id")`

* `editForm` es un formulario.
* `.elements` es una colección (como un array) que tiene todos los campos del formulario.
* `.namedItem("id")` busca un campo específico por su `name="id"`.

Ejemplo HTML:

```html
<form id="editUserForm">
  <input type="text" name="id" />
  <input type="text" name="name" />
  <input type="number" name="age" />
  <input type="email" name="email" />
  <select name="status">
    <option value="active">Activo</option>
    <option value="inactive">Inactivo</option>
  </select>
</form>
```

Entonces `editForm.elements.namedItem("name")` va a devolverte el `<input name="name" />`.

---

### 🔹 ¿Por qué usamos `as HTMLInputElement` o `as HTMLSelectElement`?

Porque TypeScript no sabe automáticamente qué tipo de campo es (input, select, etc).

Para poder usar cosas como `.value`, tenemos que indicárselo manualmente:

```ts
const nameInput = editForm.elements.namedItem("name") as HTMLInputElement
```

Y si es un `<select>`, usamos:

```ts
const statusInput = editForm.elements.namedItem("status") as HTMLSelectElement
```

Así TypeScript sabe que puede hacer:

```ts
nameInput.value       // texto
statusInput.value     // valor seleccionado del <select>
```

---

## 🧪 Ejemplo visual

```ts
const editForm = document.getElementById("editUserForm") as HTMLFormElement

const nameInput = editForm.elements.namedItem("name") as HTMLInputElement

console.log(nameInput.value) // muestra el nombre que está cargado en el input
nameInput.value = "Nuevo nombre" // cambia el valor que se muestra en el input
```

---

## ✅ ¿Cuándo y por qué usar esta forma?

* ✅ Cuando ya tenés el formulario y querés llenar los campos (como en una edición).
* ✅ Cuando querés validar o trabajar con campos individuales (acceder a `value`, marcar errores, etc.).
* ✅ Es **más directo y seguro** que hacer `document.getElementById(...)` por cada campo.

---

Finalmente hay que agregar el eventListener en caso de que el formulario de edición se haga `submit`

#### 📄 /src/views/userCreateForm.ts
```ts
import { createRepositories } from "../api/factory/createRepositories"
import type { UpdateUser } from "../types/userType"
import { renderTable } from "./userTable"

const {userRepository} = createRepositories()

const editForm = document.getElementById("editUserForm") as HTMLFormElement


export async function setUserEditFormEvents() {
  // Editar usuario
  editForm.addEventListener("submit", async (e) => {
    e.preventDefault()
    const formData = new FormData(editForm)
    const id = formData.get("id") as string
    const input = {
      name: formData.get("name"),
      age: Number(formData.get("age")),
      email: formData.get("email"),
      status: formData.get("status")
    } as UpdateUser

    const result = await userRepository.updateUser(id, input)
    if (result.success) {
      alert("Usuario actualizado")
      editForm.style.display = "none"
      editForm.reset()
      renderTable()
    } else {
      alert(result.error)
    }
  })
}
```
