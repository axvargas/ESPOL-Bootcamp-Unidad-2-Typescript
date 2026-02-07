# 📡 Proyecto Final

## **Monitoreo de Antennas en Guayaquil**

---

## 🧩 Contexto del proyecto

La Municipalidad de Guayaquil necesita una aplicación web sencilla para **monitorear antenas de telecomunicaciones** distribuidas en la ciudad.
El sistema debe permitir **consultar información técnica**, **visualizar las antenas en un mapa** y **gestionar su visualización** desde una tabla interactiva.

Para este proyecto se trabajará con:

* **TypeScript (Vanilla)**
* **Fetch API**
* **MockAPI.io** (como backend simulado)
* **DataTables.net**
* **Mapbox GL JS**

---

## 🎯 Objetivo general

Desarrollar una aplicación web que:

1. Consuma datos de antenas desde una API externa.
2. Muestre la información en una tabla interactiva.
3. Permita visualizar y ocultar antenas en un mapa.
4. Muestre información relevante de cada antena en un popup.

---

## 📦 Fuente de datos

Los datos deben obtenerse mediante `fetch` desde la siguiente URL base:

[https://6823c58065ba05803397d6df.mockapi.io/api/v1/antennas](https://6823c58065ba05803397d6df.mockapi.io/api/v1/antennas)

---

## 🧱 Estructura del objeto Antenna

Cada antena tiene, como mínimo, las siguientes propiedades:

* `id` string
* `name` string
* `code` string
* `operator` string
* `type` string
* `status` string (maintenance | out_of_service | active)
* `powerKw` number
* `heightMts` number
* `neighborhood` string
* `latitude` number
* `longitude` number
* `installedAt` string

---

## 🛠️ Requerimientos funcionales

### 1️⃣ Consumo de datos (Fetch)

* Realizar una petición `GET` usando `fetch`.
* Manejar correctamente promesas (`async / await`).
* Validar los datos recibidos antes de usarlos.

---

### 2️⃣ Tabla de Antennas (DataTables)

* Mostrar las antenas en una tabla usando **DataTables.net**.
* Incluir al menos las siguientes columnas:

  * Name
  * Operator
  * Type
  * Status
  * Neighborhood
  * Actions

---

### 3️⃣ Botones de acción en la tabla

En la columna **Actions**, incluir botones que permitan:

* **Show Location**: mostrar la antena en el mapa.
* **Hide Location**: ocultar la antena del mapa.

---

### 4️⃣ Mapa interactivo (Mapbox)

* Inicializar un mapa centrado en Guayaquil. API KEY: `pk.eyJ1IjoiYW5kcmVzeGF2aWVyOTkiLCJhIjoiY20zbWUyMWdqMTFzZDJrcHhidjlhZjFwaCJ9.JxyJSYQBmQI77epaw4xUaQ`
* Colocar marcadores (`Marker`) usando latitude y longitude.
* Al hacer **pasar el mouse por encima del marcador**, mostrar un **Popup** con información relevante:

  * Name
  * Operator
  * Type
  * Status
  * Power (kW)
  * Neighborhood

---

### 5️⃣ Organización del código

* Usar TypeScript correctamente tipado.
* Separar responsabilidades (API, lógica, UI, mapa).
```
src/
 ├─ api/
 │   └─ antennas.api.ts
 ├─ types/
 │   └─ antenna.type.ts
 ├─ map/
 │   └─ map.ts
 ├─ main.ts
index.html
```
* Evitar variables `any`.

---

## 🚫 Restricciones

* No usar frameworks (React, Vue, Angular).
* No modificar los datos directamente en el frontend (solo lectura).
* No usar librerías adicionales no vistas en clase.

---

## 📁 Entregables

* Código fuente del proyecto.
* Enlace al repositorio (GitHub, GitLab, etc.).
* README explicando:

  * cómo ejecutar el proyecto
  * qué hace cada parte principal

---

# 📝 Rúbrica de Evaluación

| Criterio                       | Descripción                                                                  | Puntaje     |
| ------------------------------ | ---------------------------------------------------------------------------- | ----------- |
| **Fetch y consumo de API**     | Uso correcto de `fetch`, `async/await`, manejo de promesas y errores básicos | 20 pts      |
| **Tipado en TypeScript**       | Uso correcto de types/interfaces, sin `any`, variables tipadas               | 15 pts      |
| **Uso de DataTables**          | Tabla correctamente configurada, columnas claras, datos visibles             | 15 pts      |
| **Botones de acción**          | Botones funcionales para mostrar/ocultar antenas                             | 10 pts      |
| **Integración con Mapbox**     | Mapa correctamente inicializado y centrado en Guayaquil                      | 15 pts      |
| **Markers y Popups**           | Marcadores visibles y popup con información relevante                        | 10 pts      |
| **Organización del código**    | Código legible, modular y bien estructurado                                  | 10 pts      |
| **Buenas prácticas generales** | Nombres claros, lógica comprensible, orden                                   | 5 pts       |
| **Total**                      |                                                                              | **100 pts** |

---

NOTA: El proyecto deben presentarlo el día Sábado a modo de Demo. Y voy a realizar una pregunta acerca del código en donde voy a pedir que me expliquen algún método o funcionalidad de alguna función de una librería que estén usando, por lo que tienen que tener bien claro para que sirve el código que están escribiendo.


