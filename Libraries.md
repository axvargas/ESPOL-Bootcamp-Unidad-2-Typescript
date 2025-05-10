# Librerías de terceros

Primero vamos a usar una librería para dar ids llamada UUID
---

## 🧩 Paso 1: Inicializar un proyecto Node con TypeScript

Abre la terminal en la carpeta donde está tu archivo `.ts` y corre:

```bash
npm init -y
```

Esto creará un archivo `package.json`, que es necesario para instalar cualquier librería como `uuid`.

---

## 🧩 Paso 2: Instalar TypeScript y uuid

```bash
npm install typescript uuid
```

Y luego instala los tipos de uuid para que TypeScript entienda cómo usarlo:

```bash
npm install --save-dev @types/uuid
```

---

## 🧩 Paso 3: Crear archivo de configuración TypeScript

Si aún no tienes un `tsconfig.json`, créalo con:

```bash
npx tsc --init
```

Esto configura cómo se compilarán tus archivos `.ts`. Puedes usar la configuración por defecto al principio.

---

## 🧩 Paso 4: Usar `uuid` en tu archivo TypeScript

Ahora puedes usar `uuid` así:

```ts
import { v4 as uuidv4 } from 'uuid'

const id = uuidv4()
console.log("New UUID:", id)
```

Recuerda que todas las librerías tienen documentación por lo que es importante ver que más nos ofrece: https://www.npmjs.com/package/uuid

---

## 🧩 Paso 5: Compilar y ejecutar tu archivo

Primero compílalo:

```bash
npx tsc tuArchivo.ts
```

Esto generará un archivo `tuArchivo.js`. Luego, ejecútalo con Node:

```bash
node tuArchivo.js
```

---


