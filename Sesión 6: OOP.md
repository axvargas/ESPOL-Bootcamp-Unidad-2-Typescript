## 🎓 Parte 1: Conceptos básicos de OOP con la clase `User`

### ✅ 1. **Clase y objetos**

Una **clase** es como un molde. Un **objeto** es una instancia de esa clase.

```ts
class User {
  name: string;
  age: number;

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }
}

const user1 = new User('Alice', 25);
```

---

### ✅ 2. **Encapsulamiento**

Usamos propiedades privadas (`#`) o protegidas (`private`) y accedemos mediante métodos públicos.

```ts
class User {
  #password: string;

  constructor(public name: string, public age: number, password: string) {
    this.#password = password;
  }

  checkPassword(input: string): boolean {
    return input === this.#password;
  }

  changePassword(current: string, newPassword: string) {
    if (this.checkPassword(current)) {
      this.#password = newPassword;
    }
  }
}
```

---

### ✅ 3. **Métodos**

Funciones dentro de la clase. Son las acciones del objeto.

```ts
class User {
  constructor(public name: string, public age: number) {}

  greet(): string {
    return `Hola, soy ${this.name} y tengo ${this.age} años`;
  }
}
```

---

### ✅ 4. **Herencia**

Una clase puede **extender** otra y heredar sus atributos y métodos.

```ts
class Admin extends User {
  role: string = 'admin';

  deleteUser(user: User) {
    console.log(`Eliminando al usuario ${user.name}`);
  }
}
```

---

### ✅ 5. **Polimorfismo**

Distintas clases pueden implementar el mismo método de forma diferente.

```ts
class Guest extends User {
  greet(): string {
    return `Soy un invitado, mi nombre es ${this.name}`;
  }
}
```
---
### Nota para los constructores:

Este código
```ts
class User {
  public name: string;
  public age: number;
  #password: string;

  constructor(name: string, age: number, password: string) {
    this.name = name;
    this.age = age;
    this.#password = password;
  }
}
```

Se puede reducir en:
```ts
class User {
  constructor(
    public name: string,
    public age: number,
    private password: string
  ) {}

  getPassword(): string {
    return this.password;
  }
}
```
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

