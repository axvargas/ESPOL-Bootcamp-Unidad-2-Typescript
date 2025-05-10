Continuación del App de restaurante:

1. CHALLENGE:
   - Haz que podamos usar una variable global para llevar el control de nextHamburguerId
   - Utiliza el mismo truco que usamos con nextOrderId++ cuando llames a addNewHamburguer.
   - Actualiza los elementos del menú para que también usen esta variable, de modo que no tengamos que ingresar manualmente los ids del 1 al 4 como lo estamos haciendo actualmente.
  
2. CHALLENGE:
   - Arregla la función addNewHamburguer usando el Utility Type Omit. 
   - Esto podría requerir más que solo cambiar el parámetro `hamburguerObj` que está tipado como "Hamburguer".
   - Devuelve el nuevo objeto hamburguesa (con el id agregado) desde la función.
  
3. CHALLENGE:
   - Agrega la función updateHamburguer usando el Utility Type Partial.
   - Devuelve el objeto de hamburguesa editado
  
4. Continuando con el código podemos usar Generics para realizar el siguiente ejercicio:
   - Agrega Generics a la función para que funcione de manera tal que la función funcione para agregar elementos a cualquier arreglo
```ts
function addToArray(array, item) {
    array.push(item)
    return array
}

// example usage:
addToArray(menu, {id: nextHamburgerId++, name: "Chicken Bacon Ranch", price: 12 })
addToArray(orderQueue, { id: nextOrderId++, hamburguer: menu[2], status: "completed" })
```
