### 🧠 ¿Por qué usar **TypeScript** en lugar de solo **JavaScript**?

Aunque JavaScript funciona bien y lo usamos todo el tiempo, tiene un problema: **no nos avisa cuando cometemos errores hasta que el programa ya está corriendo**.

#### 👎 JavaScript:
```js
let edad = "25";
console.log(edad * 2); // Resultado: 50 😨 ¿por qué? era un string
```

#### 👍 TypeScript:
```ts
let edad: number = "25"; // ❌ Error: el string no se puede asignar a un number
```

---

### 🚀 Ventajas de TypeScript

1. ✅ **Detecta errores antes de ejecutar**
   - Te avisa mientras escribes el código.
   - Reduce bugs en producción.

2. 🧩 **Mejor experiencia en el editor (VSCode)**
   - Autocompletado inteligente.
   - Documentación de funciones y objetos al instante.
   - Recomendaciones y refactorización más fácil.

3. 🧼 **El código es más claro**
   - Si ves `nombre: string`, sabes lo que se espera.
   - Ayuda a otros (o a ti mismo en el futuro) a entender tu código más rápido.

4. 🛡️ **Es más seguro**
   - Obliga a pensar qué tipo de datos manejamos.
   - Evita errores típicos de pasar datos incorrectos entre funciones.
   - Promueve el **Código defensivo**
     
5. 🧠 **Preparación para proyectos reales**
   - La mayoría de aplicaciones React en empresas usan TypeScript.
   - Cada vez más equipos lo exigen por productividad y mantenimiento.

---

### 🎯 En resumen:
> **TypeScript no reemplaza a JavaScript**, lo mejora.
>  
> Es como tener un amigo que te corrige antes de que el profesor vea tu tarea ✏️
---

## **Tipos básicos en TypeScript**
| **Tipo**       | **Ejemplo**                   |
|-----------------|-------------------------------|
| **string**      | `let name: string = "Ana";`  |
| **number**      | `let age: number = 25;`      |
| **boolean**     | `let isStudent: boolean = true;` |
| **any**         | `let random: any = "Hola";`  |
| **array**       | `let numbers: number[] = [1, 2, 3];` |

---

## **Objetos en TypeScript**
### **Definir tipos para objetos**
```typescript
let user: { name: string; age: number } = {
  name: "María",
  age: 25
};

console.log(`El usuario ${user.name} tiene ${user.age} años.`);
```

### **Con interfaces**
- Las interfaces permiten definir estructuras reutilizables para objetos.
```typescript
interface User {
  name: string;
  age: number;
}

let user: User = { name: "Pedro", age: 40 };
console.log(`El usuario ${user.name} tiene ${user.age} años.`);
```
