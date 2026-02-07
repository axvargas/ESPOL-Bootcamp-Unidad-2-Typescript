## Mini taller: Gallery de imágenes con TypeScript + API (GET)

### Objetivo

Construir una galería sencilla que:

* haga `fetch` a una API gratuita de imágenes
* muestre las imágenes en una grilla
* permita buscar por palabra clave (opcional, pero recomendado)

API recomendada (gratis, sin API key): **Picsum Photos**

* Lista de imágenes: `https://picsum.photos/v2/list?page=1&limit=12`

---

# 1) Estructura del proyecto

Usar Vite con TypeScript (vanilla):

Estructura final sugerida:

```
image-gallery/
 ├─ index.html
 ├─ src/
 │   ├─ main.ts
 │   ├─ api/
 │   │   └─ images.api.ts
 │   ├─ types/
 │   │   └─ image.type.ts
 │   └─ ui/
 │       └─ gallery.ui.ts
 └─ package.json
```

---

# 2) HTML mínimo (solo contenedores)

## `index.html`

Mantenerlo muy simple:

* un título
* un botón para cargar imágenes
* un contenedor donde se dibuja la galería

```html
<body>
  <div id="app">
    <h1>Image Gallery</h1>

    <button id="loadBtn">Load Images</button>

    <div id="gallery"></div>
  </div>

  <script type="module" src="/src/main.ts"></script>
</body>
```

CSS mínimo (en el `<head>`, dentro de `<style>`), para grilla:

```html
<style>
  #gallery {
    margin-top: 16px;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 12px;
  }
  .card {
    border: 1px solid #ddd;
    padding: 8px;
    border-radius: 6px;
  }
  .card img {
    width: 100%;
    height: 160px;
    object-fit: cover;
    border-radius: 4px;
    display: block;
  }
  .meta {
    margin-top: 6px;
    font-size: 12px;
  }
</style>
```

---

# 3) Type del dato que llega desde la API

## `src/types/image.type.ts`

Picsum devuelve objetos con estas propiedades. Se define el type para tipar el código.

```ts
export type PicsumImage = {
  id: string;
  author: string;
  width: number;
  height: number;
  url: string;
  download_url: string;
};
```

---

# 4) Función de fetch (API layer)

## `src/api/images.api.ts`

Una función simple: traer 12 imágenes.

```ts
import type { PicsumImage } from "../types/image.type";

const BASE_URL: string = "https://picsum.photos/v2/list";

export async function fetchImages(page: number, limit: number): Promise<PicsumImage[]> {
  const url: string = `${BASE_URL}?page=${page}&limit=${limit}`;

  const response: Response = await fetch(url);
  if (!response.ok) {
    throw new Error(`Failed to fetch images: ${response.status}`);
  }

  const data: PicsumImage[] = await response.json();
  return data;
}
```

---

# 5) UI: render de la galería

## `src/ui/gallery.ui.ts`

Se dibuja un “card” por cada imagen.

```ts
import type { PicsumImage } from "../types/image.type";

export function renderGallery(container: HTMLElement, images: PicsumImage[]): void {
  container.innerHTML = images
    .map((img: PicsumImage) => {
      return `
        <div class="card">
          <img src="${img.download_url}" alt="Image ${img.id}" loading="lazy" />
          <div class="meta">
            <div><strong>Author:</strong> ${img.author}</div>
            <div><strong>ID:</strong> ${img.id}</div>
          </div>
        </div>
      `;
    })
    .join("");
}
```

---

# 6) Conectar todo en `main.ts`

## `src/main.ts`

* detectar click en botón
* llamar `fetchImages`
* renderizar con `renderGallery`
* manejar error básico

```ts
import { fetchImages } from "./api/images.api";
import { renderGallery } from "./ui/gallery.ui";
import type { PicsumImage } from "./types/image.type";

const loadBtn = document.getElementById("loadBtn") as HTMLButtonElement;
const gallery = document.getElementById("gallery") as HTMLDivElement;

loadBtn.addEventListener("click", async (): Promise<void> => {
  loadBtn.disabled = true;
  loadBtn.textContent = "Loading...";

  try {
    const images: PicsumImage[] = await fetchImages(1, 12);
    renderGallery(gallery, images);
  } catch (error: unknown) {
    gallery.innerHTML = `<p style="color:#b00020;">Error loading images</p>`;
    console.log("Fetch error:", error);
  } finally {
    loadBtn.disabled = false;
    loadBtn.textContent = "Load Images";
  }
});
```
