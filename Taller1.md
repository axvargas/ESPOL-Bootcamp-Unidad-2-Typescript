# Taller 1: Zod y Módulos
---

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

### 🧩 1. Crea un esquema para `Product` (2 pts)

Requisitos del producto:

* id: UUID
* name: string
* price: number positivo
* inStock: boolean
* category: enum con "toys", "books", "electronics"

> 🎯 **Bonus**: crea el tipo `Product` usando `z.infer`. (+0.5 pt)

---


### 🧩 2. Crea `validateProductData` (2 pts)

> 🎯 **Bonus**: Utiliza mensajes personalizados para los campos que se pueda. (+0.5 pt)

---


### 🧩 3. Crea `createProduct` y `updateProduct` (4 pts)

> 🎯 **Bonus 1**:Crea una función `deleteProduct` que reciba un id y elimine el producto con ese id de la lista de productos. La función debe retornar verdadero y eliminó correctaente o falso si no encontró el id. (+1 pt)
> 🎯 **Bonus 2**:Crea una función `listProducts` que retorne la lista de products. (+0.5 pt)

---


### 🧩 4. Modulariza tu proyecto en carpetas y archivos diferentes (2 pts)
```
/types
  userTypes.ts
  productTypes.ts
/controllers
  usersController.ts
  productsController.ts
index.ts
```

---

Finalmente para poder comprimir la carpeta y subir el archivo comprimido en aula virtual debes borrar la carpeta llamada `node_modules`, luego comprimir toda la carpeta y subir el archivo en la sección del taller
