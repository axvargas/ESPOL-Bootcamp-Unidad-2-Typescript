# Taller 1: Zod y Módulos
---

## 🧠 Objetivo del Taller

Crear una pequeña **API simulada en TypeScript** que permita **crear, obtener, actualizar y eliminar productos**, usando:

✅ TypeScript
✅ Zod (para validaciones)
✅ Fetch + MockAPI.io (para simular peticiones a una API)
✅ Buen manejo de errores
✅ Modularización en archivos `/types`, `/controllers` e `index.ts`

---

## 📁 Estructura del Proyecto

```
/types
  └── product.ts         ← Zod schema + validaciones
/controllers
  └── productController.ts ← Funciones: create, update, get, delete
index.ts                 ← Usar las funciones para probar
```


Para desarrollar este taller, crea una carpeta nueva, e inicializa un proyecto de node. Sigue los siguientes pasos:
1. Incializa un proyecto de node
    ```bash
    npm init -y
    ```
2. Instala Typescript
    ```bash
    npm install --save-dev typescript
    ```
3. Genera el archivo de configuración con:
    ```bash
    npx tsc --init
    ```
4. En la raíz del proyecto crea un archivo llamada `index.ts` para que puedas dessarollar los pasos del taller.
   
---

### 🧩 0. Crea un proyecto en https://mockapi.io/ y agregar el schema de producto(1 pts)

Requisitos del producto:

* id: string
* name: string
* price: number positivo
* inStock: boolean
* category: enum con "toys", "books", "electronics"

---

### 🧩 1. Crea un esquema para `Product` usando zod (1 pts)

---


### 🧩 2. Crea `createProduct(data: CreateProduct)` (2 pts)

---


### 🧩 3. Crea `getAllProduct()` (1 pts)

---

### 🧩 4. Crea `getAllProducts(string id)` (1 pts)

---

### 🧩 5. Crea `updateProduct(id: string, updates: UpdateProduct)` (2 pts)

---


### 🧩 6. Crea `deleteProduct(id: string)` (1 pts)

---

IMPROTANTE!!: Finalmente para poder comprimir la carpeta y subir el archivo comprimido en aula virtual debes borrar la carpeta llamada `node_modules`, luego comprimir toda la carpeta y subir el archivo en la sección del taller

NOTA: Para volver a instaler node_modules, tienen que ejecutar:
`npm i` en la terminal
