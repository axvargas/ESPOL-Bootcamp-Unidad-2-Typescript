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
   ```
 2. CHALLENGE: Crea una función llamada `addNewHamburguer` que reciba un objeto de tipo hamburguesa y la añada al menú
 3. CHALLENGE: Crea una función llamada `placeOrder` que reciba el nombre de una hamburguesa como parámetro:
    - Buscar el objeto de hamburguesa en el menú
    - Agregar un nuevo objeto de tipo orden a la lista de   `orderQueue`. Ej: `{'hamburguer': ObjetodeHamburguesa, status: 'ordered'}`
    - Retornar el nuevo objeto de orden
4. CHALLENGE: Desafío: escribe otra función llamada `completeOrder` que reciba un `orderId` como parámetro,
   busque la orden correspondiente en la cola de órdenes (`orderQueue`), y marque su estado como "completed".
   Para mayor utilidad, retorna la orden encontrada desde la función.

   Nota: debes asegurarte de que estemos agregando IDs a nuestras órdenes cuando se crean nuevas órdenes.
   Puedes usar una variable global llamada `nextOrderId` e incrementarla cada vez que se cree una nueva orden
   para simular cómo una base de datos nos asignaría IDs reales.
5. 

