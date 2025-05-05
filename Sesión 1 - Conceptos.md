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

### 🧠 ¿Qué es código defensivo?

El **código defensivo** es una forma de programar **anticipándote a los errores**. En vez de asumir que todo va a salir bien (el _happy path_), también escribes tu código preparado para cuando algo salga mal (el _sad path_).

---

### ✅ **Happy Path** (camino feliz)

Es el flujo en el que **todo funciona como se espera**.  
En tu función:

```ts
const selectedHamburguer = menu.find(hamburguerObj => hamburguerObj.name === hamburguerName)
```

Si el nombre existe en el menú, todo sale bien:

- Se encuentra la hamburguesa
- Se suma el precio al total
- Se crea y retorna la orden

---

### ❌ **Sad Path** (camino triste o de error)

Es lo que pasa cuando **algo sale mal o inesperado**.

Tu código defensivo está aquí:

```ts
if (!selectedHamburguer) {
  console.error(`${hamburguerName} does not exist in the menu`)
  return
}
```

👉 Esto es código defensivo:  
- Compruebas que la hamburguesa exista antes de intentar usarla.  
- Evitas errores como `selectedHamburguer.price` siendo `undefined`.

---

### 🧪 ¿Qué pasaría sin ese chequeo?

```ts
cashInRegister += selectedHamburguer.price // ❌ Error si es undefined
```

Te lanzarías directamente al _happy path_ sin asegurarte que todo esté listo, lo cual puede romper tu aplicación.

---

### 🛡️ En resumen:

- El **happy path** es el camino ideal.  
- El **sad path** es lo que pasa cuando algo falla.  
- El **código defensivo** es anticiparte a los errores y **proteger tu aplicación** con condiciones como `if (!selectedHamburguer)`.
  
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
```typescript
type Person = {
    name: string
    age: number
    isStudent: boolean
}

let person1: Person = {
    name: "Joe",
    age: 42,
    isStudent: true
}

let person2: Person = {
    name: "Jill",
    age: 66,
    isstudent: false
} 
```
---
## **Objetos Anidados en TypeScript**
```ts
type Address = {
    street: string
    city: string
    country: string
}

type Person = {
    name: string
    age: number
    isStudent: boolean
    address: Address
}

let person1: Person = {
    name: "Joe",
    age: 42,
    isStudent: true,
    address: {
        street: "123 Main",
        city: "Anytown", 
        country: "USA"
    }
}

let person2: Person = {
    name: "Jill",
    age: 66,
    isStudent: false,
    address: {
        street: "123 Main",
        city: "Anytown",
        country: "USA"
    }
}
```
## **Propiedades opcionales**
```ts
type Address = {
    street: string
    city: string
    country: string
}

type Person = {
    name: string
    age: number
    isStudent: boolean
    address?: Address
}

let person1: Person = {
    name: "Joe",
    age: 42,
    isStudent: true,
}

let person2: Person = {
    name: "Jill",
    age: 66,
    isStudent: false,
    address: {
        street: "123 Main",
        city: "Anytown",
        country: "USA"
    }
}

```

