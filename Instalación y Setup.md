## 🖥️GUÍA DE INSTALACIÓN — **MAC y WINDOWS**

---

### ✅ 1. Instalar **Node.js** (incluye npm)

> TypeScript se instala a través de **npm**, que viene incluido con Node.js.

#### 👉 Paso para **Mac y Windows**:
1. Ir a [https://nodejs.org](https://nodejs.org)
2. Descargar la versión **LTS (Long Term Support)** recomendada.
3. Ejecutar el instalador y seguir las instrucciones.

#### 📦 Verifica instalación:
Después de instalar, abre tu terminal (o PowerShell/Command Prompt en Windows) y escribe:

```bash
node -v
npm -v
```

Deberías ver las versiones instaladas.

---

### ✅ 2. Instalar **TypeScript**

Usaremos la instalación **global** para poder compilar archivos `.ts` desde cualquier proyecto.

#### 📦 Instalar:

```bash
npm install -g typescript
```

#### ✅ Verificar instalación:

```bash
tsc -v
```

Deberías ver algo como: `Version 5.x.x`

---

### ✅ 3. Instalar **Visual Studio Code (VSCode)**

#### 👉 Paso para **Mac y Windows**:
1. Ir a [https://code.visualstudio.com/](https://code.visualstudio.com/)
2. Descargar e instalar la versión estable para tu sistema operativo.

---

### ✅ 4. Extensiones recomendadas en VSCode

1. **ESLint** (para mantener el código limpio)
2. **Prettier** (formato automático)
3. **TypeScript Essentials** *(opcional)*
4. **Code Runner** *(opcional para ejecutar código sin compilar manualmente)*

Puedes buscar estas extensiones dentro de VSCode:
- Haz clic en el icono de cuadrados a la izquierda (Extensiones)
- Busca cada nombre y haz clic en “Instalar”

---

### ✅ 5. Crear tu primer proyecto TypeScript

1. Abre VSCode
2. Abre la terminal integrada (`Ctrl + ~` o `Cmd + ~`)
3. Ejecuta:

```bash
mkdir mi-proyecto-ts
cd mi-proyecto-ts
tsc --init  # Crea tsconfig.json
touch index.ts  # En Mac (usa `type nul > index.ts` en Windows)
```

4. Escribe algo en `index.ts`:

```ts
const saludo: string = "Hola TypeScript";
console.log(saludo);
```

5. Compila y ejecuta:

```bash
tsc index.ts
node index.js
```
