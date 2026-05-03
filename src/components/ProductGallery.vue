<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

interface MediaAsset {
  tag: string
  mediaType?: 'image' | 'video' | 'spin'
}

interface GalleryWidget {
  render: () => void
  destroy: () => void
}

declare const cloudinary: {
  galleryWidget: (config: {
    container: HTMLElement
    cloudName: string
    mediaAssets: MediaAsset[]
  }) => GalleryWidget
}

const props = withDefaults(
  defineProps<{
    cloudName: string
    mediaAssets: MediaAsset[]
  }>(),
  {
    cloudName: 'demo',
    mediaAssets: () => [
      { tag: 'electric_car_product_gallery_demo' },
      { tag: 'electric_car_product_gallery_demo', mediaType: 'video' },
      { tag: 'electric_car_360_product_gallery_demo', mediaType: 'spin' },
    ],
  },
)

const galleryContainer = ref<HTMLElement | null>(null)
let widget: GalleryWidget | null = null

onMounted(() => {
  if (typeof cloudinary === 'undefined') {
    console.error(
      'Cloudinary gallery widget script not loaded. Make sure the script tag is present in index.html.',
    )
    return
  }

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
</script>

<template>
  <div ref="galleryContainer" class="gallery-container" />
</template>

<style scoped>
.gallery-container {
  width: 100%;
  min-height: 500px;
}
</style>
