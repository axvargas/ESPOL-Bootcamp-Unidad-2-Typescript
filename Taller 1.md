## Tema: Menú de Hamburguesas + Mini Carrito

### TypeScript – Arreglos y Functions

---

## 🧠 Introducción breve: Functions en TypeScript

Una **función** es un bloque de código reutilizable que:

* recibe parámetros,
* ejecuta una lógica,
* y puede retornar un valor.

En TypeScript es importante **tipar los parámetros y el retorno**.

---

## 🧱 Datos base (NO modificar)

```ts
type Hamburguer = {
  name: string;
  price: number;
  tags: string[];
};

const menu: Hamburguer[] = [
  { name: "Classic", price: 5.5, tags: ["beef", "classic"] },
  { name: "Cheese", price: 6.0, tags: ["beef", "cheese"] },
  { name: "Bacon", price: 7.25, tags: ["beef", "bacon"] },
  { name: "Double", price: 8.5, tags: ["beef", "double"] },
  { name: "Veggie", price: 6.75, tags: ["veggie"] },
  { name: "BBQ", price: 7.8, tags: ["beef", "bbq"] },
  { name: "Spicy", price: 6.9, tags: ["beef", "spicy"] },
  { name: "Mushroom", price: 7.1, tags: ["beef", "mushroom"] },
  { name: "Chicken", price: 6.4, tags: ["chicken"] },
  { name: "Deluxe", price: 9.2, tags: ["beef", "premium"] },
];

const cart: Hamburguer[] = [];
```

---

## 🔟 Funciones de arreglos a usar (obligatorias)

1. `map`
2. `filter`
3. `find`
4. `some`
5. `every`
6. `sort`
7. `slice`
8. `includes`
9. `forEach`
10. `push`

---

## 📋 Ejercicios

> ⚠️ Todas las funciones deben estar **dentro de funciones de TypeScript**

---

### 1️⃣ `map`

Función que devuelva un arreglo con **los nombres de las hamburguesas**.

---

### 2️⃣ `filter`

Función que devuelva las hamburguesas con precio menor o igual a un valor dado.

---

### 3️⃣ `find`

Función que busque una hamburguesa por nombre.

---

### 4️⃣ `some`

Función que verifique si existe alguna hamburguesa con un precio menor a un valor dado.

---

### 5️⃣ `every`

Función que valide que **todas** las hamburguesas tengan un precio mayor a `0`.

---

### 6️⃣ `sort`

Función que devuelva el menú ordenado por precio (ascendente), **sin modificar el original**.

---

### 7️⃣ `slice`

Función que permita **paginar el menú**, recibiendo página y tamaño de página.

---

### 8️⃣ `includes` (USANDO TAGS)

Función que reciba un **tag** (por ejemplo `"beef"` o `"veggie"`) y devuelva solo las hamburguesas que tengan ese tag.

📌 Pista: cada hamburguesa tiene un arreglo `tags`.

---

## 🛒 Mini Carrito

### 9️⃣ `push`

Función que agregue una hamburguesa al carrito.

---

### 🔟 `forEach`

Función que imprima en consola el contenido del carrito con este formato:

```
- Classic: $5.5
```

---

## 📦 Entrega

* Archivo: `menu.ts`
* Debe incluir:

  * `type Hamburguer`
  * `menu` y `cart`
  * Todas las funciones
  * `console.log` para probar cada ejercicio

---
