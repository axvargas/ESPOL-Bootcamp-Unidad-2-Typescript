## 🧩 ¿Qué es una **interface**?

Una **interface** en TypeScript define **la forma que debe tener un objeto o una clase**, pero **no implementa lógica**.

> 👉 Es como un contrato: “si usas esta interfaz, debes tener estas propiedades o métodos”.

### 📦 Ejemplo:

```ts
interface Animal {
  name: string;
  makeSound(): void;
}

class Dog implements Animal {
  name: string;

  constructor(name: string) {
    this.name = name;
  }

  makeSound() {
    console.log("Woof!");
  }
}
```

---

## 🧱 ¿Qué es una **clase abstracta**?

Una **clase abstracta** es una **plantilla base** que puede tener:

* Métodos **abstractos** (sin lógica).
* Métodos **con lógica** reutilizable.

> 👉 No se puede crear una instancia de una clase abstracta. Solo se puede extenderla.

### 🧪 Ejemplo:

```ts
abstract class Animal {
  constructor(public name: string) {}

  abstract makeSound(): void;

  move() {
    console.log(`${this.name} está caminando`);
  }
}

class Cat extends Animal {
  makeSound() {
    console.log("Meow!");
  }
}
```

---

## 🔍 Diferencias clave

| Característica                | Interface                 | Clase Abstracta                |
| ----------------------------- | ------------------------- | ------------------------------ |
| Puede tener propiedades       | ✅                         | ✅                              |
| Puede tener lógica            | ❌                         | ✅ (en métodos no abstractos)   |
| Puede implementar varias      | ✅ (puedes usar múltiples) | ❌ (solo se puede extender una) |
| Se instancia directamente     | ❌                         | ❌                              |
| ¿Se usa para objetos también? | ✅                         | ❌ (solo clases)                |

---

## 🧠 ¿Cuál se usa más?

* En **proyectos React con TypeScript**, las **interfaces son más comunes**:

  * Para definir tipos de props.
  * Para modelos de datos (`User`, `Product`, etc.).
  * Para contratos ligeros sin lógica.

* Las **clases abstractas** se usan más en proyectos donde hay una arquitectura OOP más fuerte, como backend en NestJS, o lógica compleja con herencia.
---

Ahora podemos hacer uso de patrones de diseño que usan los conceptos aprendidos con aterioridad, para ordernear de manera profesional nuestros proyectos que involucran datos en APIs para ello vamos a hacer uso de 
- DataSource Pattern
- Repository Prattern
- Factory Pattern

Siguiendo la siguiente estructura:

![Repository +  DataSource + Factory Pattern drawio (2)](https://github.com/user-attachments/assets/66c496ed-79cb-412e-b45d-078c58de0a8d)



