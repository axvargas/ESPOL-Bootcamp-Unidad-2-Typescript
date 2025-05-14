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


