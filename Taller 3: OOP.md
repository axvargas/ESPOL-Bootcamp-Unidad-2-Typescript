
## 🛠️ Mini taller con `Product`

### 🎯 Objetivo

Aplicar todos los conceptos de OOP creando una clase `Product`.

---

## 📦 Clase base: `Product`

### Instrucciones:

1. Crea una clase `Product` con: (2 pts)

   * `name: string`
   * `price: number`
   * `stock: number` (privado)

2. Agrega un constructor. (2 pts)

3. Agrega métodos públicos: (3 pts)

   * `getStock()`: Debe retornar el stock
   * `buy(quantity: number)`: descuenta del stock si hay suficiente, es decir, resta la cantidad quantity del stock si y solo si hay stock suficiente, sino, muestra un mensaje que diga que no hay stock, caso contrario muestra un mensaje que la venta fue exitosa
   * `restock(quantity: number)`: Sumarle la cantidad quantity al stock

4. Crea una clase hija `DigitalProduct` que: (3 pts)
   * Para el constructor hacer uso de super. Pero investiga como hacer para que se pueda inicializar la prodiedad stock con un numero infinito. PISTA: Busca en internet qué es `Infinity` en javascript
   * Siempre tenga stock "infinito"
   * `buy()` siempre funciona, pero no modifica stock. Es decir sobreescribe la función buy y esta vez solo debe mostrar por pantalla un mensaje de que la venta fue exitosa

---


## 🧪 Actividades para probar que tu taller funciona (Bonus: 5 pts)

1. Crea 3 productos y simula compras, verificando el stock.
2. Crea un `DigitalProduct` y prueba comprar varias veces.
3. Crea una función que reciba un `Product` o `DigitalProduct` y llame a `.buy()` y `.getStock()` → aquí se ve el **polimorfismo**.
4. Agrega validaciones si se intenta comprar con cantidad negativa.
5. Añade un nuevo método `getInfo()` en `Product` y sobreescríbelo en `DigitalProduct`.
