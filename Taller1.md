# Taller 1: Zod y Módulos
---

### 🧩 1. Crea un esquema para `Product` (2 pts)

Requisitos del producto:

* id: UUID
* name: string
* price: number positivo
* inStock: boolean
* category: enum con "toys", "books", "electronics"

> 🎯 **Bonus**: crea el tipo `Product` usando `z.infer`. (+1 pt)
---

### 🧩 2. Crea `validateProductData` (2 pts)

> 🎯 **Bonus**: Utiliza mensajes personalizados para los campos que se pueda. (+1 pt)
---

### 🧩 3. Crea `createProduct` y `updateProduct` (4 pts)

> 🎯 **Bonus**:Crea una función `deleteProduct` que reciba un id y elimine el producto con ese id de la lista de productos. La función debe retornar verdadero y eliminó correctaente o falso si no encontró el id. (+1 pt)
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
