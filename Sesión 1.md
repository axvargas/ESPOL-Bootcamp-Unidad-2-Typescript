---
### Restaurante de Hamburguesas:
#### Creando una aplicación de consola con Javascript:
 1. Empecemos con un menú de hamburguesas `menu`, una variable para guardar el dinero en la caja registradora `cashInRegistry` (siempre va a empezar con $100) y una variable para registrar las ordenes en curso `orderQueue`.
   ```js
     let menu = [
        { name: 'Supreme', price: 10},
        { name: 'Deluxe', price: 12},
        { name: '1/4 pound with cheese', price: 13},
        { name: 'Classic', price: 14},
        { name: 'Extreme', price: 14},
        { name: 'Salty Crunch', price: 9}
      ]
      
      let cashInRegister = 100;
      let orderQueue = [];      
   ```
 2. CHALLENGE: Crea una función llamada `addNewHamburguer` que reciba un objeto de tipo hamburguesa y la añada al menú
 3. CHALLENGE: Crea una función llamada `placeOrder` que reciba el nombre de una hamburguesa como parámetro:
    - Buscar el objeto de hamburguesa en el menú
    - Añada la ganancia a la caja registradora `cashInRegistry`
    - Agregar un nuevo objeto de tipo orden a la lista de   `orderQueue`. Ej: `{'hamburguer': ObjetodeHamburguesa, status: 'ordered'}`
    - Retornar el nuevo objeto de orden
 4. CHALLENGE: escribe otra función llamada `completeOrder` que reciba un `orderId` como parámetro,
    busque la orden correspondiente en la cola de órdenes (`orderQueue`), y marque su estado como "completed".
    Para mayor utilidad, retorna la orden encontrada desde la función.

    Nota: debes asegurarte de que estemos agregando IDs a nuestras órdenes cuando se crean nuevas órdenes.
    Puedes usar una variable llamada `const nextOrderId` e incrementarla cada vez que se cree una nueva orden
    para simular cómo una base de datos nos asignaría IDs reales.
 5. Llama a las funciones previamente creadas:
     ```js
     addNewHamburguer({ name: "Chicken Bacon", cost: 12 })
     addNewHamburguer({ name: "BBQ Deluxe", cost: 12 })
     addNewHamburguer({ name: "Spicy Sausage", cost: 11 })
      
     placeOrder("Supreme")
     completeOrder('1')
      
     console.log("Menu:", menu)
     console.log("Cash in register:", cashInRegister)
     console.log("Order queue:", orderQueue) 
     ```
#### Moviendo el código a Typescript:
Podemos observar que el que el editor de código nos muestra varios errores
1. Uno de los errores más evidentes es el que se tiene con ```nextOrderId++```, que indica que no se puede asignar otro valor a una variable declarada con ```const```
2. Typescript te obliga a siempre manejar un código defensivo, por ejemplo en ```selectedHamburguer.price``` se muestra que ```selectedHamburguer``` puede ser undefined, lo que provocaría un error, ya que no puedes acceder a una propiedad de una ```undefined```
   Por esa razón vamos a validar si exise o no
   ````ts
   function placeOrder(hamburguerName: string) {
     const selectedHamburguer = menu.find(hamburguerObj => hamburguerObj.name === hamburguerName)
     if (!selectedHamburguer) {
       console.error(`${hamburguerName} does not exist in the menu`)
       return
     }
     cashInRegister += selectedHamburguer.price
     const newOrder = { id: nextOrderId++, hamburguer: selectedHamburguer, status: "ordered" }
     orderQueue.push(newOrder)
     return newOrder
   }
   ```

#### Migrando de Javascript a Typescript:
1. CHALLENGE: Enséñale a TypeScript qué tipo de dato debe usarse para el `orderId` en la función `completeOrder`. Luego revisa si TypeScript muestra advertencias adicionales y corrígelas.
2. CHALLENGE: Crea un tipo de objeto de Hamburguesa. Debe incluir las propiedades `name` y `price`.
3. CHALLENGE: indícale a TypeScript que ```hamburguerObj``` debe ser del tipo ```Hamburguer```. Luego, revisa el código para ver si aparecen nuevas advertencias de TypeScript y corrige esos problemas.
4. CHALLENGE: Enséñale a TypeScript qué tipo de dato debe usarse para el `hamburguerName` en la función `placeOrder`. Luego revisa si TypeScript muestra advertencias adicionales y corrígelas.
5. CHALLENGE: Agrega un tipo `Order`. Debe tener las propiedades `id`, `hamburguer` y `status`. Revisa el código si necesitas un recordatorio sobre qué tipos de datos deberían tener esas propiedades.
6. CHALLENGE: Arregla el warning que arroja typescript en la función `completeOrder`, usando código defensivo.
