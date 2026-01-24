
# 🧠 ¿Qué es un Event Listener?

Un **event listener** es una forma de decirle a un programa:

> “Cuando pase **algo**, ejecuta **este código**.”

Ese “algo” se llama **evento**.

---

## 📌 Ejemplos de eventos en la vida real

* Cuando **haces click** en un botón → se ejecuta algo
* Cuando **pasas el mouse por encima** de un elemento → se ejecuta algo
* Cuando **escribes** en un input → se ejecuta algo
* Cuando **la página carga** → se ejecuta algo

💬 Frase clave para alumnos:

> “Un event listener espera a que el usuario haga algo.”

---

## 🧩 ¿Cómo funciona un event listener?

Todo event listener tiene **3 partes**:

1. **Quién escucha** el evento
2. **Qué evento** escucha
3. **Qué hacer** cuando ocurre

En palabras:

> “Elemento + Evento + Acción”

---

## 🧪 Ejemplo conceptual (sin código)

* **Elemento:** un botón
* **Evento:** click
* **Acción:** mostrar un mensaje

👉 Cuando el usuario hace click → pasa algo.

---

# 🎯 Eventos más comunes (para que entiendan el concepto)

| Evento       | ¿Cuándo ocurre?                    |
| ------------ | ---------------------------------- |
| `click`      | Cuando haces click                 |
| `mouseenter` | Cuando el mouse entra              |
| `mouseleave` | Cuando el mouse sale               |
| `mousemove`  | Cuando mueves el mouse             |
| `keydown`    | Cuando presionas una tecla         |
| `keyup`      | Cuando sueltas una tecla           |
| `submit`     | Cuando envías un formulario        |
| `load`       | Cuando la página termina de cargar |

💬 Importante:

> “No todos los eventos son del DOM.
> Las librerías también tienen sus propios eventos.”

---

# 🧩 Event listeners en librerías (como Mapbox)

Mapbox **no usa directamente el DOM**, pero sigue la **misma idea**:

> “Cuando pase algo en el mapa, ejecuta este código.”

Ejemplos en Mapbox:

* mouse sobre un marker
* click en el mapa
* mover el mapa
* hacer zoom

---

# 🗺️ PARTE PRÁCTICA – Popup al hacer hover en un Marker

## 🎯 Objetivo

Cuando el usuario **pase el mouse sobre un marker**, mostrar un popup con:

* Latitud
* Longitud

Cuando el mouse salga, el popup desaparece.

---

# 🧱 Conceptos nuevos que vamos a usar

Antes del código, explícales esto:

### 1️⃣ `Popup`

Es una ventana pequeña que aparece sobre el mapa.

### 2️⃣ `mouseenter` y `mouseleave`

* `mouseenter` → cuando el mouse entra
* `mouseleave` → cuando el mouse sale

---

# 🧩 Código explicado paso a paso

📄 **`src/map/map.ts`**

```ts
import mapboxgl from "mapbox-gl";
import { addresses } from "../data/addresses";

mapboxgl.accessToken = "pk.eyJ1IjoiYW5kcmVzeGF2aWVyOTkiLCJhIjoiY20zbWUyMWdqMTFzZDJrcHhidjlhZjFwaCJ9.JxyJSYQBmQI77epaw4xUaQ";

export function initMap(): void {
  const map = new mapboxgl.Map({
    container: "map",
    style: "mapbox://styles/mapbox/streets-v11",
    center: [-3.7038, 40.4168],
    zoom: 4,
  });

  map.addControl(new mapboxgl.NavigationControl());

  addresses.forEach(address => {
    let marker = new mapboxgl.Marker()
      .setLngLat([address.longitude, address.latitude])
      .addTo(map);

    const popup = new mapboxgl.Popup({
      closeButton: false,
      closeOnClick: false,
    })

    let description:string = `
      <strong>Coordinates</strong><br/>
      Lat: ${address.latitude}<br/>
      Lng: ${address.longitude}
    `
    // 3️⃣ Evento: cuando el mouse entra al marker
    marker.getElement().addEventListener("mouseenter", () => {
        map.getCanvas().style.cursor = 'pointer';
        popup.setLngLat([address.longitude, address.latitude]).setHTML(description).addTo(map);
    });

    // 4️⃣ Evento: cuando el mouse sale del marker
    marker.getElement().addEventListener("mouseleave", () => {
        map.getCanvas().style.cursor = '';
        popup.remove();
    });
  });
}

```

---

# 🧠 Explicación didáctica del código

## 🔹 `marker.getElement()`

* Devuelve el **elemento HTML** del marcador
* Permite usar **event listeners normales**

💬 “Ahora sí podemos escuchar eventos del mouse.”

---

## 🔹 `addEventListener("mouseenter", ...)`

* Se ejecuta **cuando el mouse entra**
* Ideal para hover

---

## 🔹 `addEventListener("mouseleave", ...)`

* Se ejecuta **cuando el mouse sale**
* Perfecto para ocultar cosas
