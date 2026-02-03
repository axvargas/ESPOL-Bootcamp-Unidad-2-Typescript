# Paso 0 — Crear el proyecto
Haciendo uso de 
```ts
npm create vite@latest
```

# Paso 1 — Schema y types de Orders (Zod) con latitude y longitude

## Concepto: Schema (Zod)

Un **schema** define las reglas de cómo deben ser los datos.
Sirve para validar datos “de verdad” cuando la app corre (no solo cuando escribes).

## Concepto: `z.infer`

Convierte el schema en un `type` de TypeScript automáticamente.
Esto evita escribir el type dos veces.

Intalar zod
```ts
npm install zod
```

📄 Crear archivo: `src/types/order.schema.ts`

```ts
import { z } from "zod";

export const OrderStatusSchema = z.enum(["pending", "paid", "shipped", "cancelled"]);

export const OrderSchema = z.object({
  id: z.string().uuid(),
  customerName: z.string().min(2, "Customer name must have at least 2 characters"),
  status: OrderStatusSchema,
  total: z.number().positive("Total must be greater than 0"),
  notes: z.string().max(120, "Notes must be 120 characters or less").optional(),
  latitude: z.number(),
  longitude: z.number(),
  createdAt: z.string(), // ISO string
});

export type Order = z.infer<typeof OrderSchema>;

/**
 * Datos de entrada del formulario:
 * - No incluye id ni createdAt porque los crea el sistema
 * - total, latitude y longitude vienen como string desde el form, por eso usamos coerce
 */
export const OrderFormSchema = z.object({
  customerName: z.string().min(2, "Customer name must have at least 2 characters"),
  status: OrderStatusSchema,
  total: z.coerce.number().positive("Total must be greater than 0"),
  notes: z.string().max(120, "Notes must be 120 characters or less").optional().or(z.literal("")),
  latitude: z.coerce.number(),
  longitude: z.coerce.number(),
});

export type OrderFormData = z.infer<typeof OrderFormSchema>;
```

**Qué se logró aquí**

* `Order` (final) tiene `id` y `createdAt`.
* `OrderFormData` (entrada) no tiene `id` ni `createdAt`.
* `z.coerce.number()` convierte strings del form a número.

---

# Paso 2 — HTML: Formulario + CDN CSS de DataTables + tabla vacía

## Concepto: `name` en formularios

`FormData` obtiene valores usando el atributo `name`.
Sin `name`, `FormData.get("...")` no encuentra nada.

📄 `index.html` (raíz del proyecto)

**2.1** Agregar el CDN de estilos de DataTables en el `<head>`:

```html
<link rel="stylesheet" href="https://cdn.datatables.net/2.0.8/css/dataTables.dataTables.min.css" />
```

**2.2** Crear el formulario (con `name`) y la tabla vacía (`id="myTable"`):

```html
<form id="orderForm">
  <div>
    <label>Customer Name</label>
    <input id="customerName" name="customerName" placeholder="Ej: Ana Pérez" />
  </div>

  <div>
    <label>Status</label>
    <select id="status" name="status">
      <option value="pending">pending</option>
      <option value="paid">paid</option>
      <option value="shipped">shipped</option>
      <option value="cancelled">cancelled</option>
    </select>
  </div>

  <div>
    <label>Total</label>
    <input id="total" name="total" type="number" step="0.01" placeholder="Ej: 19.99" />
  </div>

  <div>
    <label>Notes (optional)</label>
    <input id="notes" name="notes" placeholder="Ej: Entregar en la tarde" />
  </div>

  <div>
    <label>Latitude</label>
    <input id="latitude" name="latitude" type="number" step="0.000001" placeholder="Ej: -2.189412" />
  </div>

  <div>
    <label>Longitude</label>
    <input id="longitude" name="longitude" type="number" step="0.000001" placeholder="Ej: -79.889066" />
  </div>

  <button type="submit">Create Order</button>
</form>

<hr />

<table id="myTable"></table>
```

**Qué se logró aquí**

* El formulario ya tiene campos mínimos para crear una orden.
* Existe un `<table id="myTable">` para que DataTables la controle.
* El CSS de DataTables ya está listo para estilos.

---

# Paso 3 — Leer el formulario con `new FormData()` y `formData.get(...)`

## Concepto: FormData

`FormData` lee todos los campos del formulario que tengan `name`.
`formData.get("campo")` devuelve `string | File | null`.

Instalar uuid:
```ts
npm install uuid
```

📄 En `src/main.ts` (agregar esto al inicio)

**3.1** Importar types y schemas:

```ts
import { v4 as uuidv4 } from "uuid";
import { OrderFormSchema, OrderSchema, type Order, type OrderFormData } from "./types/order.schema";
```

**3.2** Obtener el form usando `document.getElementById` (tipado):

```ts
const form = document.getElementById("orderForm") as HTMLFormElement;
```

**3.3** Crear una función para leer valores con FormData:

```ts
function readFormData(form: HTMLFormElement): {
  customerName: string;
  status: string;
  total: string;
  notes: string;
  latitude: string;
  longitude: string;
} {
  const formData = new FormData(form);

  return {
    customerName: String(formData.get("customerName") ?? ""),
    status: String(formData.get("status") ?? ""),
    total: String(formData.get("total") ?? ""),
    notes: String(formData.get("notes") ?? ""),
    latitude: String(formData.get("latitude") ?? ""),
    longitude: String(formData.get("longitude") ?? ""),
  };
}
```

```ts
{
  customerName: string;
  status: string;
  total: string;
  notes: string;
  latitude: string;
  longitude: string;
}
```

Puede ser cambiado por su propio tipo agregando el siguiente codigo al final del archivo `types/order.schema.ts` 

```ts
export type OrderRaw = {
  customerName: string;
  status: string;
  total: string;
  notes: string;
  latitude: string;
  longitude: string;
}
```

Y luego utilizarlo en el main (Obviamente importandolo):

```ts
function readFormData(form: HTMLFormElement): OrderRaw {
  const formData = new FormData(form);

  return {
    customerName: String(formData.get("customerName") ?? ""),
    status: String(formData.get("status") ?? ""),
    total: String(formData.get("total") ?? ""),
    notes: String(formData.get("notes") ?? ""),
    latitude: String(formData.get("latitude") ?? ""),
    longitude: String(formData.get("longitude") ?? ""),
  };
}
```

**Qué se logró aquí**

* Se puede “leer” el formulario sin usar `.value` input por input.
* Todo se obtiene como string (después Zod convierte lo necesario).

---

Nota: Para poder probar que está pasando es necesario agregar todo desde el evento:

```ts
form.addEventListener("submit", (e: SubmitEvent) => {
  e.preventDefault();
  // Leer el formulario
  // Validar los datos del formulario
  // Crear la order
  // Guardar la orden
});
```

# Paso 4 — Validar los datos del formulario con Zod (`safeParse`)

## Concepto: `safeParse`

* `parse()` valida y si está mal **lanza error** (rompe flujo si no se captura).
* `safeParse()` valida y **no rompe**: devuelve `{ success: true, data }` o `{ success: false, error }`.

📄 En `src/main.ts` (agregar debajo)

```ts
function validateOrderForm(raw: OrderRaw): OrderFormData | null {
  const result = OrderFormSchema.safeParse(raw);

  if (!result.success) {
    console.log("Validation error:", result.error.issues);
    return null;
  }

  const data: OrderFormData = {
    ...result.data,
    notes: result.data.notes === "" ? undefined : result.data.notes,
  };

  return data;
}
```

**Qué se logró aquí**

* Si el usuario ingresa datos inválidos, se ve el error en consola.
* Si es válido, se obtiene un objeto tipado `OrderFormData`.

---

# Paso 5 — Crear una Order completa usando `uuid` (id) y fecha (`createdAt`)

## Concepto: `uuid`

Genera un `id` único que identifica la orden para futuros updates y deletes.

📄 En `src/main.ts` (agregar debajo)

```ts
function createOrderEntity(data: OrderFormData): Order {
  const candidate: Order = {
    id: uuidv4(),
    customerName: data.customerName,
    status: data.status,
    total: data.total,
    notes: data.notes,
    latitude: data.latitude,
    longitude: data.longitude,
    createdAt: new Date().toISOString(),
  };

  const safeOrder: Order = OrderSchema.parse(candidate);
  return safeOrder;
}
```

**Qué se logró aquí**

* Se construye un objeto completo `Order` con `id` y `createdAt`.
* Se valida de nuevo para asegurar que lo guardado cumple el schema final.

---

# Paso 6 — Guardar la Order en un arreglo tipado

## Concepto: Arreglo tipado

`Order[]` significa “solo se pueden guardar órdenes válidas”.

📄 En `src/main.ts` (agregar debajo)

```ts
const orders: Order[] = [];

function saveOrder(order: Order): void {
  orders.push(order);
}
```

**Qué se logró aquí**

* Las órdenes se guardan en memoria.
* Más adelante se usan para listar en DataTables.

---

# Paso 7 — Conectar todo al submit del formulario

## Concepto: submit + preventDefault

Cuando se envía un form, el navegador recarga la página.
`preventDefault()` evita esa recarga.

📄 En `src/main.ts` (agregar al final)

```ts
form.addEventListener("submit", (e: SubmitEvent) => {
  e.preventDefault();

  const raw = readFormData(form);

  const validated: OrderFormData | null = validateOrderForm(raw);
  if (!validated) return;

  const order: Order = createOrderEntity(validated);
  saveOrder(order);

  console.log("Order created:", order);
  console.log("All orders:", orders);
});
```

**Qué se logró aquí**

* Ya se pueden crear órdenes desde el formulario.
* Se ven las órdenes en consola.

---

# Paso 8 — Instalar DataTables (solo instalación + CSS ya aplicado)

## Instalación

```bash
npm install datatables.net-dt
```

**Qué se logró aquí**

* Ya se puede usar `import DataTable from 'datatables.net-dt';`
* El CSS ya está cargado por CDN en el HTML.
* La tabla existe: `<table id="myTable"></table>`

---

# Paso 10 — Usar DataTables con Orders (reemplazar `data` por `orders`)

## Objetivo

Mostrar órdenes reales del arreglo `orders` en la tabla.

📌 Primero: inicializar la tabla con `orders` vacía (arranca sin filas).
Luego, cada vez que se crea una orden, se actualiza la tabla.

**10.1** Inicializar DataTable con `orders`:

```ts
// @ts-ignore
let ordersTable = new DataTable("#myTable", {
  data: orders,
  columns: [
    { title: "Customer", data: "customerName" },
    { title: "Status", data: "status" },
    { title: "Total", data: "total" },
    { title: "Lat", data: "latitude" },
    { title: "Lng", data: "longitude" },
    {
      title: "Acciones",
      data: null,
      render: (order) => `
        <button class="edit-btn" data-id="${order.id}">Editar</button>
        <button class="delete-btn" data-id="${order.id}">Eliminar</button>
      `,
    },
  ],
});
```
## Conceptos importantes

* `data: data` → la tabla se alimenta desde un arreglo.
* `columns` → define qué se muestra y de dónde sale.
* `render` → permite dibujar botones en una columna.
* `data-id` → se usa luego para saber qué item editar/borrar.

---

**10.2** Función para refrescar DataTable cuando cambien las órdenes:

```ts
function refreshOrdersTable(): void {
  // @ts-ignore
  ordersTable.clear();
  // @ts-ignore
  ordersTable.rows.add(orders);
  // @ts-ignore
  ordersTable.draw();
}
```

**10.3** Al final del submit, después de `saveOrder(order)`, llamar refresco:

```ts
refreshOrdersTable();
```

---

# Paso 11 — Delete: borrar una orden usando el botón de la tabla

## Concepto: Event Delegation (delegación)

Los botones se crean dentro de la tabla dinámicamente.
Por eso se escucha el click en un contenedor (la tabla) y se revisa qué botón fue.

**11.1** Crear función para eliminar del arreglo:

```ts
function deleteOrderById(id: string): void {
  const index: number = orders.findIndex((o: Order) => o.id === id);
  if (index === -1) return;

  orders.splice(index, 1);
}
```

**11.2** Escuchar clicks en la tabla:

```ts
const tableElement = document.getElementById("myTable") as HTMLTableElement;

tableElement.addEventListener("click", (e: MouseEvent) => {
  const target = e.target as HTMLElement;

  if (target.classList.contains("delete-btn")) {
    const id: string = String(target.getAttribute("data-id") ?? "");
    if (!id) return;

    deleteOrderById(id);
    refreshOrdersTable();
    console.log("Deleted order id:", id);
  }
});
```

### ¿Para qué sirve?

1. **Para que TypeScript permita usar `.value`** sin error.
2. **Para tener autocompletado correcto** (TypeScript sabe que es un input).
3. **Para evitar errores de tipado** cuando trabajas con el DOM.


---

# Paso 12 — Update: editar una orden usando el botón “Editar”

## Objetivo

1. Clic “Editar” → buscar orden por id
2. Llenar el formulario con esa orden
3. En el submit, si hay “modo edición”, actualizar en vez de crear

---

## 12.1 Agregar un hidden input al formulario para el id en edición

En el HTML, dentro del `<form>` arriba:

```html
<input type="hidden" id="editingId" name="editingId" />
```

---

## 12.2 Función para cargar una orden al form (rellenar inputs)

```ts
function loadOrderToForm(order: Order): void {
  (document.getElementById("editingId") as HTMLInputElement).value = order.id;
  (document.getElementById("customerName") as HTMLInputElement).value = order.customerName;
  (document.getElementById("status") as HTMLSelectElement).value = order.status;
  (document.getElementById("total") as HTMLInputElement).value = String(order.total);
  (document.getElementById("notes") as HTMLInputElement).value = order.notes ?? "";
  (document.getElementById("latitude") as HTMLInputElement).value = String(order.latitude);
  (document.getElementById("longitude") as HTMLInputElement).value = String(order.longitude);
}
```

Se usa `as` porque `document.getElementById(...)` devuelve un tipo **muy general**: `HTMLElement | null`.

* **`HTMLElement`** es “cualquier elemento HTML” (puede ser un `div`, `form`, `input`, etc.).
* Pero la propiedad **`.value`** no existe en todos los elementos, solo en elementos de formulario como **`HTMLInputElement`**, `HTMLSelectElement`, etc.

Entonces, TypeScript te obliga a aclarar:

> “Yo sé que este elemento específico es un `<input>`.”

Eso se hace con:

```ts
document.getElementById("editingId") as HTMLInputElement
```


---

## 12.3 Detectar click en “Editar” y cargar form

Dentro del listener de click de la tabla:

```ts
if (target.classList.contains("edit-btn")) {
  const id: string = String(target.getAttribute("data-id") ?? "");
  if (!id) return;

  const order: Order | undefined = orders.find((o: Order) => o.id === id);
  if (!order) return;

  loadOrderToForm(order);
  console.log("Editing order:", order);
}
```

---

## 12.4 Cambiar el submit para “Create vs Update”

### Leer también el `editingId` desde FormData

Actualizar `readFormData` para incluir `editingId`:

```ts
function readFormData(form: HTMLFormElement): {
  editingId: string;
  customerName: string;
  status: string;
  total: string;
  notes: string;
  latitude: string;
  longitude: string;
} {
  const formData = new FormData(form);

  return {
    editingId: String(formData.get("editingId") ?? ""),
    customerName: String(formData.get("customerName") ?? ""),
    status: String(formData.get("status") ?? ""),
    total: String(formData.get("total") ?? ""),
    notes: String(formData.get("notes") ?? ""),
    latitude: String(formData.get("latitude") ?? ""),
    longitude: String(formData.get("longitude") ?? ""),
  };
}
```

### En submit: si `editingId` existe, se actualiza

Crear función update por id:

```ts
function updateOrderById(id: string, data: OrderFormData): void {
  const index: number = orders.findIndex((o: Order) => o.id === id);
  if (index === -1) return;

  const current: Order = orders[index];

  const updated: Order = {
    ...current,
    customerName: data.customerName,
    status: data.status,
    total: data.total,
    notes: data.notes,
    latitude: data.latitude,
    longitude: data.longitude,
  };

  orders[index] = updated;
}
```

Modificar submit:

```ts
form.addEventListener("submit", (e: SubmitEvent) => {
  e.preventDefault();

  const raw = readFormData(form);

  const validated: OrderFormData | null = validateOrderForm(raw);
  if (!validated) return;

  if (raw.editingId) {
    updateOrderById(raw.editingId, validated);

    // limpiar modo edición
    (document.getElementById("editingId") as HTMLInputElement).value = "";

    console.log("Order updated:", raw.editingId);
    refreshOrdersTable();
    form.reset();
    return;
  }

  const order: Order = createOrderEntity(validated);
  saveOrder(order);

  console.log("Order created:", order);
  refreshOrdersTable();
  form.reset();
});
```

---

## 12.5 Ajustar validateOrderForm para ignorar editingId

`OrderFormSchema` no incluye `editingId`, por eso se valida solo el objeto con campos de orden.

Para validar, enviar solo lo que importa:

```ts
const validated: OrderFormData | null = validateOrderForm({
  customerName: raw.customerName,
  status: raw.status,
  total: raw.total,
  notes: raw.notes,
  latitude: raw.latitude,
  longitude: raw.longitude,
});
```

# Ajustes extras:

## Parte 1 — Mostrar errores de Zod debajo del formulario (lista)

### Objetivo

* Cuando Zod detecte errores: mostrar una lista debajo del formulario.
* Cuando todo esté correcto: ocultar/limpiar esa lista.

---

### 1.1 Modificación en `index.html`

Agregar un contenedor para errores **debajo** del formulario (después del botón, antes de cerrar `</form>`).

```html
<!-- Debajo del botón Create/Update -->
<ul id="zodErrors"></ul>
```

Recomendado: agregar un poco de estilo para que se vea claro (en el `<style>` del HTML):

```html
<style>
  #zodErrors {
    margin-top: 12px;
    padding-left: 18px;
    color: #b00020;
  }
  #zodErrors.hidden {
    display: none;
  }
</style>
```

**Qué se logró**

* Existe un lugar fijo en la interfaz para mostrar errores de validación.
* Con la clase `hidden` se puede ocultar completamente.

---

### 1.2 Modificación en `src/main.ts`

Agregar referencias al contenedor de errores usando `document.getElementById`:

```ts
const zodErrorsEl = document.getElementById("zodErrors") as HTMLUListElement;
```

Agregar funciones para mostrar/limpiar errores:

```ts
function clearZodErrors(): void {
  zodErrorsEl.innerHTML = "";
  zodErrorsEl.classList.add("hidden");
}

function showZodErrors(messages: string[]): void {
  zodErrorsEl.innerHTML = messages.map((msg: string) => `<li>${msg}</li>`).join("");
  zodErrorsEl.classList.remove("hidden");
}
```

Actualizar `validateOrderForm` para que use el listado de errores:

```ts
function validateOrderForm(raw: OrderRaw): OrderFormData | null {
  const result = OrderFormSchema.safeParse(raw);

  if (!result.success) {
    const messages: string[] = result.error.issues.map((issue) => issue.message);
    showZodErrors(messages);
    return null;
  }

  clearZodErrors();

  const data: OrderFormData = {
    ...result.data,
    notes: result.data.notes === "" ? undefined : result.data.notes,
  };

  return data;
}
```

**Qué se logró**

* Los errores de Zod ya no se ven solo en consola.
* El usuario entiende qué campos están mal.
* Si el formulario es válido, desaparecen los errores.

---

## Parte 2 — Mensajes arriba del formulario por 5 segundos (Create/Update/Delete)

### Objetivo

* Mostrar un mensaje arriba del formulario cuando:

  * se crea una orden correctamente
  * se actualiza correctamente
  * se elimina correctamente
* Ese mensaje debe desaparecer solo en **5 segundos**.

---

### 2.1 Modificación en `index.html`

Agregar un contenedor de mensajes **arriba** del formulario:

```html
<div id="toastMessage"></div>
```

Ubicarlo justo antes de `<form id="orderForm">`.

Agregar estilos:

```html
<style>
  #toastMessage {
    margin: 12px 0;
    padding: 10px;
    border-radius: 6px;
    display: none;
  }
  #toastMessage.success {
    display: block;
    background: #e8f6ea;
    color: #0a7a0a;
  }
  #toastMessage.error {
    display: block;
    background: #fde8ec;
    color: #b00020;
  }
</style>
```

**Qué se logró**

* Existe una zona arriba del formulario para mensajes “tipo notificación”.
* Se puede mostrar/ocultar sin mover el layout.

---

### 2.2 Modificación en `src/main.ts`

Agregar referencia al elemento del mensaje:

```ts
const toastMessageEl = document.getElementById("toastMessage") as HTMLDivElement;
```

Agregar una variable para controlar el timeout:

```ts
let toastTimeoutId: number | null = null;
```

Agregar función para mostrar el mensaje por 5 segundos:

```ts
function showToast(text: string, kind: "success" | "error" = "success"): void {
  toastMessageEl.textContent = text;
  toastMessageEl.className = kind; // aplica clase success o error

  if (toastTimeoutId !== null) {
    window.clearTimeout(toastTimeoutId);
  }

  toastTimeoutId = window.setTimeout((): void => {
    toastMessageEl.textContent = "";
    toastMessageEl.className = "";
    toastMessageEl.style.display = "none";
    toastTimeoutId = null;
  }, 5000);

  toastMessageEl.style.display = "block";
}
```

> Nota: aquí se usa `style.display = "block"` para mostrarlo, y se limpia al ocultarlo.

**Qué se logró**

* Se puede mostrar un mensaje temporal sin depender de consola.
* Si se disparan mensajes seguidos, el anterior se reemplaza correctamente.

---

## Parte 3 — Confirmación al eliminar (alert sencillo)

### Objetivo

Antes de eliminar, pedir confirmación al usuario.

---

### 3.1 Modificación en el evento de click del botón Delete

En el listener donde se detecta `delete-btn`, agregar confirmación:

```ts
if (target.classList.contains("delete-btn")) {
  const id: string = String(target.getAttribute("data-id") ?? "");
  if (!id) return;

  const confirmed: boolean = window.confirm("¿Estás seguro de eliminar esta orden?");
  if (!confirmed) return;

  deleteOrderById(id);
  refreshOrdersTable();

  showToast("Orden eliminada correctamente", "success");
}
```

**Qué se logró**

* Evita eliminaciones accidentales.
* El usuario confirma antes de borrar.

---

## Parte 4 — Mensajes de Create y Update usando el “toast” + limpiar errores

### Objetivo

* Cuando Create funciona: mostrar “Orden guardada correctamente”.
* Cuando Update funciona: mostrar “Orden actualizada correctamente”.
* Al éxito: limpiar errores de Zod y limpiar modo edición.

---

### 4.1 Modificación en el submit (Create vs Update)

Ubicación: dentro del `form.addEventListener("submit", ...)`

Después de `validateOrderForm(...)` y cuando todo es válido, asegurar:

* Si crea → `showToast("Orden guardada correctamente", "success")`
* Si actualiza → `showToast("Orden actualizada correctamente", "success")`
* Siempre que sea válido → `clearZodErrors()`

Ejemplo de ajustes en el flujo:

**En Update:**

```ts
if (raw.editingId) {
  updateOrderById(raw.editingId, validated);

  (document.getElementById("editingId") as HTMLInputElement).value = "";

  refreshOrdersTable();
  form.reset();

  clearZodErrors();
  showToast("Orden actualizada correctamente", "success");
  return;
}
```

**En Create:**

```ts
const order: Order = createOrderEntity(validated);
saveOrder(order);

refreshOrdersTable();
form.reset();

clearZodErrors();
showToast("Orden guardada correctamente", "success");
```

**Qué se logró**

* El usuario recibe feedback claro cuando todo fue correcto.
* Se refuerza la importancia de validación: si hay errores, aparecen; si no, desaparecen.

---

# Introducción breve: ¿qué es `fetch` y para qué se usa?

`fetch` es una función del navegador que permite hacer **peticiones HTTP** a un servidor o a una API.
En otras palabras, sirve para “hablar” con un backend para:

* **GET**: leer datos (traer órdenes)
* **POST**: crear datos (crear una orden)
* **PUT/PATCH**: actualizar datos (editar una orden)
* **DELETE**: borrar datos (eliminar una orden)

En un CRUD real, las órdenes no viven en un arreglo en memoria, sino en una base de datos. Para simular eso sin tener que construir un backend propio, se puede usar **MockAPI.io**, que ofrece endpoints listos para practicar.

### ¿Cómo se usa `fetch`?

`fetch(url, options)` devuelve una **Promise** (una promesa de que “algo llegará después”).
Por eso normalmente se usa con `async/await`:

* `await fetch(...)` espera la respuesta del servidor
* `response.json()` convierte la respuesta a un objeto/array de JavaScript

Ejemplo mental simple:

1. “Pido datos” (fetch)
2. “Espero la respuesta”
3. “Convierto a JSON”
4. “Uso los datos en mi app”

---

# Paso a paso: agregar MockAPI + `fetch` al proyecto

## Meta de esta etapa

Reemplazar el arreglo en memoria como “fuente de verdad” por una API real (MockAPI).
El flujo será:

1. Al cargar la app: **traer órdenes desde la API** (GET)
2. Al crear: **guardar en la API** (POST) y refrescar la tabla/arreglo
3. Al actualizar: **actualizar en la API** (PUT) y refrescar
4. Al eliminar: **eliminar en la API** (DELETE) y refrescar

> Las validaciones con Zod se mantienen igual: se valida antes de enviar al backend y también se valida la respuesta.

---

## Paso 1 — Crear el recurso en MockAPI.io

1. Entrar a MockAPI.io y crear un proyecto.

2. Crear un recurso llamado: `orders`

3. Agregar campos (MockAPI crea `id` automáticamente como string):

   * `customerName` (string)
   * `status` (string)
   * `total` (number)
   * `notes` (string)
   * `latitude` (number)
   * `longitude` (number)
   * `createdAt` (string)

4. Copiar la base URL, por ejemplo:

* `https://xxxxx.mockapi.io`

El endpoint final quedará así:

* `https://xxxxx.mockapi.io/orders`

> Importante: MockAPI genera su propio `id`. En esta etapa se puede mantener `uuid` para entender IDs, pero lo más real es que el backend genere el ID. Se decide más abajo.

---

## Paso 2 — Guardar la URL de la API en una constante

### Cambio a hacer en `src/main.ts`

Agregar al inicio:

```ts
const API_BASE_URL: string = "https://xxxxx.mockapi.io";
const ORDERS_ENDPOINT: string = `${API_BASE_URL}/orders`;
```

**Para qué se hace**

* Evita repetir strings largos en el código.
* Si mañana cambia la URL, se cambia en un solo lugar.

---

## Paso 3 — Ajuste del schema de Zod para aceptar IDs del backend

### Idea clave

MockAPI genera `id` como string. Eso encaja con el schema actual (`id: z.string()`), así que no hay problema.

Pero hay un detalle: si se sigue usando `uuid` para generar IDs, al hacer POST con MockAPI, MockAPI puede ignorarlo o reemplazarlo (depende de la configuración). La forma más simple para enseñar es:

* En **Create**: no enviar `id`, que lo genere MockAPI
* En **Update/Delete**: usar el `id` que regresa MockAPI

### Cambio a hacer en `src/types/order.schema.ts`

Separar `OrderCreateSchema` (sin id) para enviar al backend.

Agregar debajo de `OrderFormSchema`:

```ts
export const OrderCreateSchema = z.object({
  customerName: z.string().min(2),
  status: OrderStatusSchema,
  total: z.number().positive(),
  notes: z.string().optional(),
  latitude: z.number(),
  longitude: z.number(),
  createdAt: z.string(),
});

export type OrderCreate = z.infer<typeof OrderCreateSchema>;
```

**Para qué se hace**

* `Order` es lo que existe “guardado” (ya tiene id).
* `OrderCreate` es lo que se envía para crear (sin id).

> Zod sirve para que el “contrato” sea claro: qué se envía y qué se recibe.

---

## Paso 4 — Crear funciones HTTP (GET, POST, PUT, DELETE) en un archivo separado

### Por qué separar

Si se mezcla lógica de fetch con lógica de UI en el mismo archivo, se vuelve difícil de leer. Separar ayuda a que cada parte tenga un propósito.

### Crear archivo: `src/api/orders.api.ts`

```ts
import { OrderSchema, type Order, OrderCreateSchema, type OrderCreate } from "../types/order.schema";

const API_BASE_URL: string = "https://xxxxx.mockapi.io";
const ORDERS_ENDPOINT: string = `${API_BASE_URL}/orders`;

/**
 * GET /orders
 * Trae todas las órdenes.
 */
export async function apiGetOrders(): Promise<Order[]> {
  const response: Response = await fetch(ORDERS_ENDPOINT);

  if (!response.ok) {
    throw new Error(`GET orders failed: ${response.status}`);
  }

  const data: unknown = await response.json();

  // Validar que sea un array de Order
  if (!Array.isArray(data)) {
    throw new Error("Invalid orders response: expected array");
  }

  const parsed: Order[] = data.map((item: unknown) => OrderSchema.parse(item));
  return parsed;
}

/**
 * POST /orders
 * Crea una orden.
 */
export async function apiCreateOrder(payload: OrderCreate): Promise<Order> {
  // Validación extra antes de enviar
  const safePayload: OrderCreate = OrderCreateSchema.parse(payload);

  const response: Response = await fetch(ORDERS_ENDPOINT, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(safePayload),
  });

  if (!response.ok) {
    throw new Error(`POST order failed: ${response.status}`);
  }

  const data: unknown = await response.json();
  const created: Order = OrderSchema.parse(data);
  return created;
}

/**
 * PUT /orders/:id
 * Actualiza una orden.
 */
export async function apiUpdateOrder(id: string, payload: OrderCreate): Promise<Order> {
  const safePayload: OrderCreate = OrderCreateSchema.parse(payload);

  const response: Response = await fetch(`${ORDERS_ENDPOINT}/${id}`, {
    method: "PUT",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(safePayload),
  });

  if (!response.ok) {
    throw new Error(`PUT order failed: ${response.status}`);
  }

  const data: unknown = await response.json();
  const updated: Order = OrderSchema.parse(data);
  return updated;
}

/**
 * DELETE /orders/:id
 * Elimina una orden.
 */
export async function apiDeleteOrder(id: string): Promise<void> {
  const response: Response = await fetch(`${ORDERS_ENDPOINT}/${id}`, {
    method: "DELETE",
  });

  if (!response.ok) {
    throw new Error(`DELETE order failed: ${response.status}`);
  }
}
```

**Qué se está usando y para qué**

* `async/await`: escribir código asíncrono como si fuera lineal
* `response.ok`: indica si la petición fue exitosa (status 200-299)
* `headers` y `Content-Type`: decir que se envía JSON
* `JSON.stringify`: convertir objeto JS a texto JSON para enviarlo
* `response.json()`: convertir respuesta JSON a objeto JS
* `Zod.parse`: validar que lo recibido del backend tiene la forma correcta

---

## Paso 5 — Reemplazar el arreglo local por datos de la API al iniciar

### Cambio a hacer en `src/main.ts`

1. Importar funciones de API
2. Al iniciar, cargar órdenes con GET y llenar el arreglo

Agregar imports:

```ts
import { apiGetOrders, apiCreateOrder, apiUpdateOrder, apiDeleteOrder } from "./api/orders.api";
import { type Order, type OrderFormData, OrderFormSchema, type OrderCreate } from "./types/order.schema";
```

Asegurar que `orders` siga existiendo como estado local para DataTables y el UI:

```ts
const orders: Order[] = [];
```

Crear una función para “sincronizar” desde backend:

```ts
async function loadOrdersFromApi(): Promise<void> {
  const remoteOrders: Order[] = await apiGetOrders();

  orders.length = 0;
  orders.push(...remoteOrders);

  refreshOrdersTable();
}
```

**Para qué se hace**

* `orders` sigue siendo el estado en el frontend.
* La fuente real ahora es la API.
* `loadOrdersFromApi()` se llama al iniciar.

Finalmente, llamar al inicio:

```ts
loadOrdersFromApi().catch((err: unknown) => {
  console.log("Error loading orders:", err);
});
```

---

## Paso 6 — Create: enviar POST en vez de guardar en arreglo

### Idea

1. Validar form con Zod
2. Construir payload (sin id)
3. `await apiCreateOrder(payload)`
4. Agregar el resultado al arreglo y refrescar tabla

Crear una función para convertir `OrderFormData` en `OrderCreate`:

```ts
function buildCreatePayload(data: OrderFormData): OrderCreate {
  return {
    customerName: data.customerName,
    status: data.status,
    total: data.total,
    notes: data.notes,
    latitude: data.latitude,
    longitude: data.longitude,
    createdAt: new Date().toISOString(),
  };
}
```

En el submit (en la parte de Create), reemplazar la lógica:

```ts
const payload: OrderCreate = buildCreatePayload(validated);

apiCreateOrder(payload)
  .then((created: Order) => {
    orders.unshift(created);
    refreshOrdersTable();
    clearZodErrors();
    showToast("Orden guardada correctamente", "success");
    form.reset();
  })
  .catch((err: unknown) => {
    console.log("Create error:", err);
    showToast("Error al guardar la orden", "error");
  });
```

> También se puede hacer con `await` dentro de una función `async`, pero este formato es fácil de seguir al inicio.

---

## Paso 7 — Update: enviar PUT en vez de actualizar el arreglo

En la parte donde se hace update, reemplazar:

```ts
const payload: OrderCreate = buildCreatePayload(validated);

apiUpdateOrder(raw.editingId, payload)
  .then((updated: Order) => {
    const index: number = orders.findIndex((o: Order) => o.id === updated.id);
    if (index !== -1) orders[index] = updated;

    refreshOrdersTable();
    clearZodErrors();
    showToast("Orden actualizada correctamente", "success");
    (document.getElementById("editingId") as HTMLInputElement).value = "";
    form.reset();
  })
  .catch((err: unknown) => {
    console.log("Update error:", err);
    showToast("Error al actualizar la orden", "error");
  });
```

---

## Paso 8 — Delete: confirmar y luego hacer DELETE en la API

En el click del botón delete:

```ts
if (target.classList.contains("delete-btn")) {
  const id: string = String(target.getAttribute("data-id") ?? "");
  if (!id) return;

  const confirmed: boolean = window.confirm("¿Estás seguro de eliminar esta orden?");
  if (!confirmed) return;

  apiDeleteOrder(id)
    .then(() => {
      const index: number = orders.findIndex((o: Order) => o.id === id);
      if (index !== -1) orders.splice(index, 1);

      refreshOrdersTable();
      showToast("Orden eliminada correctamente", "success");
    })
    .catch((err: unknown) => {
      console.log("Delete error:", err);
      showToast("Error al eliminar la orden", "error");
    });
}
```

---

# Cierre: qué se gana al usar fetch + MockAPI

* El CRUD ya no depende de un arreglo temporal que se pierde al recargar.
* Se practica el flujo real de una app:

  * Validar (Zod)
  * Enviar (fetch)
  * Recibir respuesta
  * Actualizar UI/estado


