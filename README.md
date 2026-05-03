# Cloudinary Product Gallery — Vue Demo

A minimal Vue 3 application showing how to integrate the [Cloudinary Product Gallery Widget](https://cloudinary.com/documentation/product_gallery_reference) into a Vue project. The demo renders images, video, and 360° spin assets sourced from Cloudinary tags.

## What this demonstrates

- Loading the Cloudinary Product Gallery Widget script via `index.html`
- Wrapping the widget in a reusable Vue component (`ProductGallery.vue`)
- Initialising the widget with `onMounted` and targeting a native DOM element via a template ref
- Cleaning up the widget with `onUnmounted` to avoid memory leaks
- Passing `cloudName` and `mediaAssets` into the component as props

## Prerequisites

- [Node.js](https://nodejs.org/) 18 or later
- A Cloudinary account — [sign up free](https://cloudinary.com/users/register/free)

No additional npm packages are required. The gallery widget is loaded directly from Cloudinary's CDN.

## Getting started

### 1. Clone the repo

```bash
git clone https://github.com/cloudinary-devs/cld-vue-product-gallery.git
cd cld-vue-product-gallery
```

### 2. Install dependencies

```bash
npm install
```

### 3. Set your cloud name

Open `src/App.vue` and replace `'demo'` with your own Cloudinary cloud name:

```typescript
const cloudName = 'your-cloud-name'
```

Update `mediaAssets` to reference your own tags, or keep the demo tags to see the electric car example straight away.

### 4. Run the app

```bash
npm run dev
```

Navigate to `http://localhost:5173/` — the gallery will render automatically.

## Project structure

```
├── index.html                        # Loads the Cloudinary gallery widget script
└── src/
    ├── main.ts                       # Vue app entry point
    ├── App.vue                       # Root component — sets cloudName & mediaAssets
    ├── style.css                     # Global styles
    └── components/
        └── ProductGallery.vue        # Reusable gallery component
```

## How the component works

The widget script exposes a global `cloudinary` object. `ProductGallery.vue` calls `cloudinary.galleryWidget()` after the component is mounted and destroys it when the component is unmounted:

```typescript
const galleryContainer = ref<HTMLElement | null>(null)
let widget: GalleryWidget | null = null

onMounted(() => {
  widget = cloudinary.galleryWidget({
    container: galleryContainer.value!,
    cloudName: props.cloudName,
    mediaAssets: props.mediaAssets,
  })
  widget.render()
})

onUnmounted(() => {
  widget?.destroy()
})
```

The component accepts two props so it can be reused anywhere in your app:

| Prop | Type | Description |
|---|---|---|
| `cloudName` | `string` | Your Cloudinary cloud name |
| `mediaAssets` | `MediaAsset[]` | Array of tag-based asset definitions |

Each asset entry supports `tag` (required) and an optional `mediaType` of `'image'` (default), `'video'`, or `'spin'`.

## Useful links

- [Product Gallery Widget documentation](https://cloudinary.com/documentation/product_gallery_reference)
- [Product Gallery Widget demo](https://demo.cloudinary.com/product-gallery)
- [Cloudinary Vue SDK](https://cloudinary.com/documentation/vue_integration)
- [Cloudinary sign-up](https://cloudinary.com/users/register/free)

## Building for production

```bash
npm run build
```

Output is written to the `dist/` directory, optimized for production.
