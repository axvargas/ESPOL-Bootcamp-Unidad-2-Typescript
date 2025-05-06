## 🔗 ¿Qué son las **Unions** en TypeScript?

Una **union** te permite que una variable tenga **más de un tipo posible**.  
Usamos el símbolo `|` (barra vertical) para decir: "**puede ser esto o aquello**".

---

### 🧠 Ejemplo básico:

```ts
let respuesta: string | number;

respuesta = "Aprobado";  // ✅
respuesta = 10;          // ✅
respuesta = true;        // ❌ Error: no es string ni number
```

➡️ Aquí, `respuesta` puede ser **una cadena o un número**, pero nada más.

---

### 🎯 ¿Para qué sirven?

- Cuando una variable puede tener **dos formas válidas**.
- Cuando un parámetro puede aceptar diferentes tipos.
- Para evitar usar `any`, pero seguir siendo flexibles.

---

### 🧪 Ejemplo con funciones:

```ts
function imprimirId(id: number | string) {
  console.log(`Tu ID es: ${id}`);
}
```

Aquí `id` puede ser un número (`123`) o un texto (`"abc123"`), y TypeScript lo permite sin que pierdas seguridad de tipos.

---

### 🛑 ¡Ojo! No puedes usar propiedades específicas de un tipo sin verificarlo primero:

```ts
function imprimirId(id: number | string) {
  // console.log(id.toUpperCase()); ❌ Error si es número

  if (typeof id === "string") {
    console.log(id.toUpperCase()); // ✅ Solo si es string
  }
}
```
