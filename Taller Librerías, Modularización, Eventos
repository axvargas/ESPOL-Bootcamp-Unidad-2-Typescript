# 🧑‍🏫 Taller

## Proyecto con **Vite + TypeScript + Zod + Mapbox**

### Mini proyecto: **Mostrar direcciones (Addresses) en un mapa**

---

## 🎯 Objetivo del taller (explicado para alumnos)

> “Vamos a crear un proyecto real de frontend, como los que se hacen en empresas, usando:
>
> * TypeScript
> * librerías externas
> * validación de datos
> * y un mapa interactivo”

No es solo aprender código, es **aprender a trabajar en un proyecto profesional**.

---

# 🧩 PARTE 1 – Crear el proyecto con Vite

## ¿Qué es Vite?

Vite es una herramienta moderna que:

* crea proyectos frontend rápidamente
* compila TypeScript
* levanta un servidor local
* recarga los cambios automáticamente

💬 Frase clave para alumnos:

> “Vite nos ahorra configurar todo a mano.”

---

## 🛠 Crear el proyecto

En la terminal:

```bash
npm create vite@latest
```

### ¿Qué hace exactamente este comando?

Vamos parte por parte:

* `npm create`
  → ejecuta un generador de proyectos

* `vite@latest`
  → usa la versión más reciente de Vite


📌 Resultado:

```txt
my-app/
```

---

## Entrar al proyecto e instalar dependencias base

```bash
cd my-app
npm install
```

### ¿Qué hace `npm install`?

* descarga las dependencias del proyecto
* crea la carpeta `node_modules`

💬 Frase clave:

> “Sin `node_modules`, el proyecto no funciona.”

---

## Levantar el proyecto

```bash
npm run dev
```

➡️ Abre un servidor local
➡️ Normalmente en `http://localhost:5173`

---

# ⚙️ PARTE 2 – Entender el proyecto generado por Vite

Estructura inicial:

```txt
my-app/
├─ index.html
├─ src/
│  └─ main.ts
├─ tsconfig.json
├─ package.json
└─ vite.config.ts
```

### Conceptos importantes

* `index.html` → HTML principal
* `main.ts` → punto de entrada de TypeScript
* `tsconfig.json` → reglas de TypeScript
* `vite.config.ts` → configuración de Vite

💬 Frase clave:

> “Todo empieza en `main.ts`.”

---

# 📦 PARTE 3 – Instalar librerías externas

## ¿Qué es una dependencia?

Es código que **no escribimos nosotros**, pero usamos.

En este proyecto usaremos:

* **Zod** → validar datos
* **Mapbox** → mostrar mapas

---

## Instalar Zod

```bash
npm install zod
```

### ¿Para qué sirve Zod?

Zod se usa para:

* definir la forma de los datos
* validar datos en tiempo de ejecución
* evitar errores cuando los datos están mal

💬 Ejemplo real:

> “Datos que vienen de una API pueden venir incompletos o mal.”

---

## Instalar Mapbox

```bash
npm install mapbox-gl
```

### ¿Para qué sirve Mapbox?

Mapbox sirve para:

* mostrar mapas interactivos
* usar coordenadas (latitud y longitud)
* dibujar puntos, marcadores, rutas

---

## Instalar tipos de Mapbox (TypeScript)

```bash
npm install -D @types/mapbox-gl
```

📌 Explicación clara:

* Mapbox está escrito en JavaScript
* TypeScript necesita ayuda para entenderlo
* `@types/...` provee esa ayuda

---

# 🧱 PARTE 4 – Definir datos con Zod

## ¿Por qué no solo usar `type`?

Porque `type`:

* solo existe al escribir código

Zod:

* valida **cuando la app corre**

---

## Crear el esquema Address

📄 `src/types/address.schema.ts`

```ts
import { z } from "zod";

export const AddressSchema = z.object({
  id: z.string(),
  street: z.string(),
  city: z.string(),
  latitude: z.number(),
  longitude: z.number(),
});

export type Address = z.infer<typeof AddressSchema>;
```

### Funciones usadas aquí

* `z.object` → define un objeto
* `z.string()` / `z.number()` → validaciones
* `z.infer` → crea el tipo TS automáticamente

💬 Frase clave:

> “Zod define los datos, TypeScript aprende de Zod.”

---

## Datos sin validar (raw data)

📄 `src/data/addresses.ts`

```ts
import { AddressSchema, Address } from "../types/address.schema";

const rawAddresses = [
  { id: "1", street: "Av. Siempre Viva 742", city: "Madrid", latitude: 40.4168, longitude: -3.7038 },
  { id: "2", street: "Calle Mayor 15", city: "Lisboa", latitude: 38.7223, longitude: -9.1393 },
  { id: "3", street: "Jr. de la Unión 100", city: "Lima", latitude: -12.0464, longitude: -77.0428 },
  { id: "4", street: "Carrera 7", city: "Bogotá", latitude: 4.711, longitude: -74.0721 },
  { id: "5", street: "Av. Corrientes", city: "Buenos Aires", latitude: -34.6037, longitude: -58.3816 },
  { id: "6", street: "Av. Providencia", city: "Santiago", latitude: -33.4489, longitude: -70.6693 },
  { id: "7", street: "Insurgentes Sur", city: "Ciudad de México", latitude: 19.4326, longitude: -99.1332 },
  { id: "8", street: "5th Avenue", city: "New York", latitude: 40.7128, longitude: -74.006 },
  { id: "9", street: "Gran Vía", city: "Madrid", latitude: 40.4203, longitude: -3.7058 },
  { id: "10", street: "Av. Paulista", city: "São Paulo", latitude: -23.5617, longitude: -46.656 },
  { id: "11", street: "Champs-Élysées", city: "Paris", latitude: 48.8698, longitude: 2.3078 },
  { id: "12", street: "Oxford Street", city: "London", latitude: 51.5154, longitude: -0.141 },
  { id: "13", street: "Shibuya Crossing", city: "Tokyo", latitude: 35.6595, longitude: 139.7005 },
];

export const addresses: Address[] = rawAddresses.map(addr =>
  AddressSchema.parse(addr)
);
```

### Funciones explicadas

* `map` → transforma datos crudos en datos validados
* `parse` → valida o lanza error

---

# 🗺 PARTE 5 – Mapbox

## ¿Qué funciones de Mapbox usamos y para qué?

### `new mapboxgl.Map`

* Crea el mapa
* Define centro y zoom

### `new mapboxgl.Marker`

* Crea un marcador
* Representa una dirección

### `.setLngLat`

* Define posición del marcador
* **Orden importante**: `[longitud, latitud]`

### `.addTo(map)`

* Dibuja el marcador en el mapa

---

## Código del mapa

📄 `src/map/map.ts`

```ts
import mapboxgl from "mapbox-gl";
import { addresses } from "../data/addresses";

mapboxgl.accessToken = "TU_TOKEN_DE_MAPBOX";

export function initMap(): void {
  const map = new mapboxgl.Map({
    container: "map",
    style: "mapbox://styles/mapbox/streets-v11",
    center: [-3.7038, 40.4168],
    zoom: 4,
  });

  addresses.forEach(address => {
    new mapboxgl.Marker()
      .setLngLat([address.longitude, address.latitude])
      .addTo(map);
  });
}
```

### Funciones usadas

* `forEach` → recorrer direcciones
* `setLngLat` → posicionar marcador
* `addTo` → mostrar en el mapa

---

## Entrada principal

📄 `src/main.ts`

```ts
import { initMap } from "./map/map";

initMap();
```

---
