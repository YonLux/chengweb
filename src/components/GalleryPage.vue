<template>
  <div class="gallery-page" :class="themeClass" :key="hobby.id">
    <div class="gallery-header">
      <button class="back-btn" @click="handleClose">
        <i class="fas fa-arrow-left"></i>
        返回
      </button>
      <h1 class="gallery-title">
        <i :class="hobby.icon" :style="{ color: hobby.color }"></i>
        {{ hobby.name }}
      </h1>
      <p class="gallery-subtitle">{{ hobby.description }}</p>
    </div>

    <div class="gallery-controls">
      <button 
        v-for="mode in viewModes" 
        :key="mode.id"
        class="view-mode-btn"
        :class="{ active: currentViewMode === mode.id }"
        @click="currentViewMode = mode.id"
      >
        <i :class="mode.icon"></i>
        {{ mode.name }}
      </button>
    </div>

    <div class="gallery-container" :class="'mode-' + currentViewMode">
      <div 
        class="gallery-grid"
        :class="'grid-' + currentViewMode"
      >
        <div
          v-for="(image, index) in images"
          :key="image.id"
          class="gallery-item"
          :style="getItemStyle(index)"
          @click="openLightbox(index)"
        >
          <div class="image-wrapper">
            <img :src="image.src" :alt="image.title" loading="lazy" />
            <div class="image-overlay">
              <h3>{{ image.title }}</h3>
              <p>{{ image.date }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>

    <Transition name="lightbox">
      <div v-if="lightboxOpen" class="lightbox" @click.self="closeLightbox">
        <button class="lightbox-close" @click="closeLightbox">
          <i class="fas fa-times"></i>
        </button>
        <button class="lightbox-prev" @click="prevImage">
          <i class="fas fa-chevron-left"></i>
        </button>
        <button class="lightbox-next" @click="nextImage">
          <i class="fas fa-chevron-right"></i>
        </button>
        
        <div class="lightbox-content">
          <Transition :name="lightboxTransition" mode="out-in">
            <img 
              :key="currentImageIndex"
              :src="images[currentImageIndex]?.src" 
              :alt="images[currentImageIndex]?.title"
              class="lightbox-image"
            />
          </Transition>
          <div class="lightbox-info">
            <h2>{{ images[currentImageIndex]?.title }}</h2>
            <p>{{ images[currentImageIndex]?.description }}</p>
            <span class="lightbox-counter">{{ currentImageIndex + 1 }} / {{ images.length }}</span>
          </div>
        </div>

        <div class="lightbox-thumbnails">
          <div
            v-for="(image, index) in images"
            :key="image.id"
            class="thumbnail"
            :class="{ active: index === currentImageIndex }"
            @click="currentImageIndex = index"
          >
            <img :src="image.src" :alt="image.title" />
          </div>
        </div>
      </div>
    </Transition>
  </div>
</template>

<script setup>
import { ref, computed, watch, onMounted, onUnmounted } from 'vue'

const props = defineProps({
  hobby: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['close'])

const viewModes = [
  { id: 'masonry', name: '瀑布流', icon: 'fas fa-th' },
  { id: 'grid', name: '网格', icon: 'fas fa-th-large' },
  { id: 'carousel', name: '轮播', icon: 'fas fa-images' },
  { id: 'spiral', name: '螺旋', icon: 'fas fa-spinner' }
]

const currentViewMode = ref('masonry')
const lightboxOpen = ref(false)
const currentImageIndex = ref(0)
const images = ref([])
const slideDirection = ref('left')

const themeClass = computed(() => `theme-${props.hobby.id}`)

const lightboxTransition = computed(() => {
  return slideDirection.value === 'left' ? 'slide-left' : 'slide-right'
})

const loadImages = async () => {
  const folder = props.hobby.id
  const baseUrl = import.meta.env.BASE_URL || '/'
  
  try {
    const response = await fetch(`${baseUrl}images/images.json`)
    const config = await response.json()
    const imageFiles = config[folder] || []
    
    const imageList = imageFiles.map((filename, index) => ({
      id: index + 1,
      src: `${baseUrl}images/${folder}/${filename}`,
      title: `${props.hobby.name} ${index + 1}`,
      description: `记录${props.hobby.name}的美好瞬间`,
      date: '2024-01-' + String(15 - index).padStart(2, '0')
    }))
    
    images.value = imageList
  } catch (error) {
    console.error('Failed to load images config:', error)
    images.value = []
  }
}

const getItemStyle = (index) => {
  if (currentViewMode.value === 'masonry') {
    const heights = [250, 350, 300, 400, 280, 320, 380, 260, 340, 290, 370, 310]
    return { height: `${heights[index % heights.length]}px` }
  }
  if (currentViewMode.value === 'spiral') {
    const angle = index * 30
    const radius = 100 + index * 20
    const x = Math.cos(angle * Math.PI / 180) * radius
    const y = Math.sin(angle * Math.PI / 180) * radius
    return {
      transform: `translate(${x}px, ${y}px) rotate(${angle}deg)`,
      zIndex: index
    }
  }
  return {}
}

const openLightbox = (index) => {
  currentImageIndex.value = index
  lightboxOpen.value = true
}

const closeLightbox = () => {
  lightboxOpen.value = false
}

const prevImage = () => {
  slideDirection.value = 'right'
  currentImageIndex.value = (currentImageIndex.value - 1 + images.value.length) % images.value.length
}

const nextImage = () => {
  slideDirection.value = 'left'
  currentImageIndex.value = (currentImageIndex.value + 1) % images.value.length
}

const handleKeydown = (e) => {
  if (!lightboxOpen.value) return
  if (e.key === 'Escape') closeLightbox()
  if (e.key === 'ArrowLeft') prevImage()
  if (e.key === 'ArrowRight') nextImage()
}

const handleClose = () => {
  emit('close')
}

watch(() => props.hobby, async () => {
  await loadImages()
  currentViewMode.value = 'masonry'
  lightboxOpen.value = false
  currentImageIndex.value = 0
}, { immediate: true })

onMounted(() => {
  window.addEventListener('keydown', handleKeydown)
})

onUnmounted(() => {
  window.removeEventListener('keydown', handleKeydown)
})
</script>

<style scoped>
.gallery-page {
  min-height: 100vh;
  padding: 20px;
  position: relative;
  overflow-x: hidden;
}

.theme-photography {
  background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%);
}

.theme-basketball {
  background: linear-gradient(135deg, #1a1a1a 0%, #2d1f1f 50%, #4a2c2a 100%);
}

.theme-volleyball {
  background: linear-gradient(135deg, #0a192f 0%, #112240 50%, #1d3557 100%);
}

.theme-soccer {
  background: linear-gradient(135deg, #0d1b2a 0%, #1b263b 50%, #2d4a3e 100%);
}

.theme-novel {
  background: linear-gradient(135deg, #1a1a2e 0%, #2d1b4e 50%, #4a1259 100%);
}

.theme-bicycle {
  background: linear-gradient(135deg, #0d1f22 0%, #1a3a3a 50%, #2d5a4e 100%);
}

.theme-pingpong {
  background: linear-gradient(135deg, #1a0a0a 0%, #2d1515 50%, #4a2020 100%);
}

.theme-running {
  background: linear-gradient(135deg, #1a1400 0%, #2d2200 50%, #4a3800 100%);
}

.gallery-header {
  text-align: center;
  padding: 40px 20px;
  position: relative;
}

.back-btn {
  position: absolute;
  left: 20px;
  top: 20px;
  background: rgba(255, 255, 255, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.2);
  color: white;
  padding: 12px 25px;
  border-radius: 30px;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 10px;
  font-size: 1rem;
  transition: all 0.3s ease;
  backdrop-filter: blur(10px);
}

.back-btn:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: translateX(-5px);
}

.gallery-title {
  font-size: 3rem;
  margin-bottom: 15px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 20px;
}

.gallery-title i {
  font-size: 2.5rem;
  animation: bounce 2s ease-in-out infinite;
}

@keyframes bounce {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-10px); }
}

.gallery-subtitle {
  color: rgba(255, 255, 255, 0.7);
  font-size: 1.2rem;
}

.gallery-controls {
  display: flex;
  justify-content: center;
  gap: 15px;
  margin-bottom: 40px;
  flex-wrap: wrap;
}

.view-mode-btn {
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  color: rgba(255, 255, 255, 0.7);
  padding: 12px 25px;
  border-radius: 25px;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 0.95rem;
  transition: all 0.3s ease;
}

.view-mode-btn:hover {
  background: rgba(255, 255, 255, 0.1);
  border-color: rgba(255, 255, 255, 0.3);
}

.view-mode-btn.active {
  background: linear-gradient(135deg, var(--primary-color), var(--secondary-color));
  border-color: transparent;
  color: white;
  box-shadow: 0 5px 20px rgba(108, 92, 231, 0.4);
}

.gallery-container {
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 20px;
}

.gallery-grid {
  display: grid;
  gap: 20px;
}

.grid-masonry {
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
}

.grid-grid {
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
}

.grid-carousel {
  display: flex;
  overflow-x: auto;
  scroll-snap-type: x mandatory;
  padding: 20px 0;
}

.grid-carousel::-webkit-scrollbar {
  height: 8px;
}

.grid-carousel::-webkit-scrollbar-thumb {
  background: var(--primary-color);
  border-radius: 4px;
}

.grid-spiral {
  display: block;
  position: relative;
  height: 800px;
}

.gallery-item {
  position: relative;
  border-radius: 15px;
  overflow: hidden;
  cursor: pointer;
  transition: all 0.5s cubic-bezier(0.4, 0, 0.2, 1);
  animation: fadeInUp 0.6s ease-out forwards;
  opacity: 0;
}

.gallery-item:nth-child(1) { animation-delay: 0.05s; }
.gallery-item:nth-child(2) { animation-delay: 0.1s; }
.gallery-item:nth-child(3) { animation-delay: 0.15s; }
.gallery-item:nth-child(4) { animation-delay: 0.2s; }
.gallery-item:nth-child(5) { animation-delay: 0.25s; }
.gallery-item:nth-child(6) { animation-delay: 0.3s; }
.gallery-item:nth-child(7) { animation-delay: 0.35s; }
.gallery-item:nth-child(8) { animation-delay: 0.4s; }
.gallery-item:nth-child(9) { animation-delay: 0.45s; }
.gallery-item:nth-child(10) { animation-delay: 0.5s; }
.gallery-item:nth-child(11) { animation-delay: 0.55s; }
.gallery-item:nth-child(12) { animation-delay: 0.6s; }

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(30px) scale(0.95);
  }
  to {
    opacity: 1;
    transform: translateY(0) scale(1);
  }
}

.gallery-item:hover {
  transform: translateY(-10px) scale(1.02);
  box-shadow: 0 25px 50px rgba(0, 0, 0, 0.4);
  z-index: 10;
}

.mode-carousel .gallery-item {
  flex: 0 0 350px;
  height: 450px;
  scroll-snap-align: center;
  margin-right: 20px;
}

.mode-spiral .gallery-item {
  position: absolute;
  width: 150px;
  height: 150px;
  left: 50%;
  top: 50%;
  margin-left: -75px;
  margin-top: -75px;
}

.image-wrapper {
  width: 100%;
  height: 100%;
  position: relative;
}

.image-wrapper img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.5s ease;
}

.gallery-item:hover .image-wrapper img {
  transform: scale(1.1);
}

.image-overlay {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 30px 20px 20px;
  background: linear-gradient(transparent, rgba(0, 0, 0, 0.8));
  transform: translateY(100%);
  transition: transform 0.4s ease;
}

.gallery-item:hover .image-overlay {
  transform: translateY(0);
}

.image-overlay h3 {
  font-size: 1.1rem;
  margin-bottom: 5px;
}

.image-overlay p {
  font-size: 0.85rem;
  color: rgba(255, 255, 255, 0.7);
}

.lightbox {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.95);
  z-index: 2000;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  backdrop-filter: blur(20px);
}

.lightbox-close {
  position: absolute;
  top: 20px;
  right: 20px;
  background: rgba(255, 255, 255, 0.1);
  border: none;
  color: white;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  font-size: 1.5rem;
  cursor: pointer;
  transition: all 0.3s ease;
}

.lightbox-close:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: rotate(90deg);
}

.lightbox-prev,
.lightbox-next {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(255, 255, 255, 0.1);
  border: none;
  color: white;
  width: 60px;
  height: 60px;
  border-radius: 50%;
  font-size: 1.3rem;
  cursor: pointer;
  transition: all 0.3s ease;
}

.lightbox-prev { left: 30px; }
.lightbox-next { right: 30px; }

.lightbox-prev:hover,
.lightbox-next:hover {
  background: var(--primary-color);
  transform: translateY(-50%) scale(1.1);
}

.lightbox-content {
  max-width: 90%;
  max-height: 70vh;
  text-align: center;
}

.lightbox-image {
  max-width: 100%;
  max-height: 70vh;
  border-radius: 15px;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.5);
}

.lightbox-info {
  margin-top: 20px;
  color: white;
}

.lightbox-info h2 {
  font-size: 1.5rem;
  margin-bottom: 10px;
}

.lightbox-info p {
  color: rgba(255, 255, 255, 0.7);
  margin-bottom: 10px;
}

.lightbox-counter {
  display: inline-block;
  background: rgba(255, 255, 255, 0.1);
  padding: 5px 15px;
  border-radius: 20px;
  font-size: 0.9rem;
}

.lightbox-thumbnails {
  position: absolute;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 10px;
  max-width: 90%;
  overflow-x: auto;
  padding: 10px;
}

.thumbnail {
  width: 60px;
  height: 60px;
  border-radius: 8px;
  overflow: hidden;
  cursor: pointer;
  border: 2px solid transparent;
  transition: all 0.3s ease;
  flex-shrink: 0;
}

.thumbnail.active {
  border-color: var(--primary-color);
  transform: scale(1.1);
}

.thumbnail img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.lightbox-enter-active,
.lightbox-leave-active {
  transition: all 0.4s ease;
}

.lightbox-enter-from,
.lightbox-leave-to {
  opacity: 0;
}

.lightbox-enter-from .lightbox-content {
  transform: scale(0.8);
}

.lightbox-leave-to .lightbox-content {
  transform: scale(0.8);
}

.slide-left-enter-active,
.slide-left-leave-active,
.slide-right-enter-active,
.slide-right-leave-active {
  transition: all 0.4s ease;
}

.slide-left-enter-from { transform: translateX(100px); opacity: 0; }
.slide-left-leave-to { transform: translateX(-100px); opacity: 0; }
.slide-right-enter-from { transform: translateX(-100px); opacity: 0; }
.slide-right-leave-to { transform: translateX(100px); opacity: 0; }

@media (max-width: 768px) {
  .gallery-title {
    font-size: 2rem;
  }

  .gallery-title i {
    font-size: 1.8rem;
  }

  .back-btn {
    position: relative;
    left: 0;
    margin-bottom: 20px;
  }

  .gallery-header {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .lightbox-prev { left: 10px; }
  .lightbox-next { right: 10px; }

  .lightbox-prev,
  .lightbox-next {
    width: 45px;
    height: 45px;
    font-size: 1rem;
  }
}
</style>
