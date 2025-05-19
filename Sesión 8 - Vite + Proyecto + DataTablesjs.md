## 🧠 **Clase 1: Vite + Proyecto TypeScript + HTML + DataTables.js + DOM**

---

## 🛠️ 1. ¿Qué es Vite?

**Vite** es una herramienta moderna para crear aplicaciones web de forma rápida. Es como un “entorno de desarrollo” que nos permite:

* Usar **módulos ES modernos** (como `import`)
* Escribir código en **TypeScript**
* Tener **recarga instantánea** al guardar archivos
* Preparar el proyecto fácilmente para producción

> ✅ Ideal para proyectos frontend que necesitan velocidad y estructura moderna

---

## 🚀 2. Crear un Proyecto Vite (TypeScript + HTML)

### ✨ Paso a paso

1. Abre una terminal y ejecuta:

```bash
npm create vite@latest my-app --template vanilla-ts
```

2. Entra al proyecto:

```bash
cd my-app
```


4. Abre el proyecto en tu editor favorito (VS Code, por ejemplo):

```bash
code .
```

---

### 🗂️ Estructura del proyecto creada por Vite

```
my-app/
├─ index.html          <-- Página HTML principal
├─ main.ts             <-- Punto de entrada del proyecto
├─ vite.config.ts      <-- Configuración de Vite
├─ tsconfig.json       <-- Configuración de TypeScript
└─ package.json
```

---

## 📦 3. Instalar DataTables.js

### ¿Qué es DataTables.js?

Es una librería que transforma una tabla HTML en una tabla **interactiva**, con:

* Paginación
* Filtro
* Ordenamiento

---

### ✅ Instalación

Desde la raíz del proyecto, ejecuta:

```bash
npm install datatables.net datatables.net-dt
```

> También puedes instalar los **types**:

```bash
npm install --save-dev @types/datatables.net
```

---

Primero vamos a copiar dentro de src las carpetas de: [api+types.zip](https://github.com/user-attachments/files/20279721/api%2Btypes.zip)

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
  <h1>Lista de Usuarios</h1>
  <table id="user-table">
  </table>
  <script type="module" src="/src/main.ts"></script>
</body>

</html>
```
Notese que se usa `<link rel="stylesheet" type="text/css" href="https://cdn.datatables.net/2.3.1/css/dataTables.dataTables.min.css">` para tener los stilos correctos de la tabla
---

#### 📄 src/views/userTable.ts

```ts
import { createRepositories } from "../api/factory/createRepositories"
import DataTable from 'datatables.net-dt'
import type { User } from "../types/userType";

const {userRepository} = createRepositories();

let datatable: DataTables.Api| undefined;

export async function renderTable() {
  const result = await userRepository.getUsers()
  if (!result.success) return alert(result.error)
  
  if (datatable) datatable.destroy()

  // @ts-ignore
  datatable = new DataTable("#user-table", {
    data: result.data,
    columns: [
      { title: "Nombre", data: "name" },
      { title: "Edad", data: "age" },
      { title: "Email", data: "email" },
      { title: "Estado", data: "status" },
      {
        title: "Acciones",
        data: null,
        render: (user: User) => `
          <button class="edit-btn" data-id="${user.id}">Editar</button>
          <button class="delete-btn" data-id="${user.id}">Eliminar</button>
        `
      }
    ]
  })
}
```
¡Claro! Vamos a explicar paso a paso qué significa esta línea de código:

```html
<button class="edit-btn" data-id="${user.id}">Editar</button>
```

---

## 🔍 ¿Qué es esto?

Es un **botón HTML** que tendrá la clase `edit-btn` y un atributo especial llamado `data-id`.

---

## 🧩 ¿Qué significa `data-id="${user.id}"`?

### ✅ Es un **atributo personalizado** (también llamado *data attribute*)

* Los atributos que comienzan con `data-` nos permiten **guardar datos personalizados** en un elemento HTML.
* En este caso, se está guardando el **ID del usuario** (`user.id`) en el botón.

Por ejemplo, si un usuario tiene este objeto:

```ts
const user = {
  id: "abc123",
  name: "Ana"
}
```

El botón generado será:

```html
<button class="edit-btn" data-id="abc123">Editar</button>
```

---

## 🎯 ¿Para qué sirve?

Sirve para **identificar fácilmente** a qué usuario pertenece ese botón cuando alguien haga clic.

---

## 📥 Cómo se usa en JavaScript

Cuando agregamos un `eventListener`, podemos **leer ese `data-id`** con `element.dataset.id`.

### Ejemplo:

```ts
document.addEventListener("click", (event) => {
  const target = event.target as HTMLElement

  if (target.classList.contains("edit-btn")) {
    const userId = target.dataset.id
    console.log("El ID del usuario que queremos editar es:", userId)
  }
})
```

---

## 📌 ¿Por qué no usar `id="abc123"` directamente?

Porque:

* El atributo `id` en HTML **debe ser único en toda la página**, y puede causar errores si se repite.
* En cambio, los `data-attributes` son seguros, flexibles y puedes tener muchos elementos con `data-id`.

---

## 🧠 Resumen

| Parte del código       | Qué hace                                       |
| ---------------------- | ---------------------------------------------- |
| `class="edit-btn"`     | Le da un nombre al botón para estilos o lógica |
| `data-id="${user.id}"` | Guarda el ID del usuario dentro del botón      |
| `${user.id}`           | Se reemplaza por el ID real del usuario        |

---

## 🔧 ¿Qué hace `addEventListener`?

El método `addEventListener` permite **escuchar eventos** en un elemento HTML.
Un evento puede ser: un clic, mover el mouse, presionar una tecla, enviar un formulario, etc.

### 📌 Sintaxis básica:

```ts
element.addEventListener("evento", funciónQueSeEjecuta)
```

### 🧠 Traducción:

> "Cuando ocurra el evento en este elemento, ejecuta esta función."

---

### 👇 Ejemplo: escuchar clics en un botón

```ts
const button = document.querySelector("button")

button?.addEventListener("click", () => {
  console.log("¡Hiciste clic!")
})
```

Cuando el usuario haga clic en el botón, se mostrará `"¡Hiciste clic!"` en la consola.

---

## 🧩 ¿Qué tipos de eventos puedo escuchar?

Aquí tienes algunos de los **event listeners más comunes**:

| Evento       | Cuándo se dispara                           |
| ------------ | ------------------------------------------- |
| `click`      | Cuando se hace clic en un elemento          |
| `submit`     | Cuando se envía un formulario               |
| `input`      | Cuando el usuario escribe en un input       |
| `change`     | Cuando cambia el valor de un input          |
| `keydown`    | Cuando se presiona una tecla                |
| `keyup`      | Cuando se suelta una tecla                  |
| `mouseenter` | Cuando el mouse entra al elemento           |
| `mouseleave` | Cuando el mouse sale del elemento           |
| `load`       | Cuando la página o imagen termina de cargar |
| `scroll`     | Cuando el usuario hace scroll               |

---


#### 📄 src/main.ts

```ts
import { renderTable, setDatatableEvents } from "./views/userTable"

// Iniciar
renderTable()
```

---
