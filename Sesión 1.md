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

5. 🧠 **Preparación para proyectos reales**
   - La mayoría de aplicaciones React en empresas usan TypeScript.
   - Cada vez más equipos lo exigen por productividad y mantenimiento.

---

### 🎯 En resumen:
> **TypeScript no reemplaza a JavaScript**, lo mejora.
>  
> Es como tener un amigo que te corrige antes de que el profesor vea tu tarea ✏️



---
#### 👎 Restaurante de Hamburguesas:
 1. Empecemos con un menú de hamburguesas `menu`, una variable para guardar el dinero en la caja registradora `cashInRegistry` (siempre va a empezar con $100) y una variable para registrar las ordenes en curso `orderQueue`.
   ```js
     menu = [
        { name: 'Supreme', price: 10},
        { name: 'Deluxe', price: 12},
        { name: '1/4 pound with cheese', price: 13},
        { name: 'Classic', price: 14},
        { name: 'Extreme', price: 14},
        { name: 'Salty Crunch', price: 9}
      ]
      
      cashInRegistry = 100;
      orderQueue = [];
      
      /* Crea una función llamada addNewPizza que reciba un objeto de tipo hamburguesa y la añada al menú */
   ```
 2. 
 3. 

