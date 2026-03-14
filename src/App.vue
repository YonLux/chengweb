<template>
  <div class="app">
    <ParticleBackground />
    
    <Transition 
      name="page-fade" 
      mode="out-in"
      @after-enter="onPageEnter"
    >
      <GalleryPage 
        v-if="currentGallery" 
        :hobby="currentGallery" 
        @close="closeGallery"
        key="gallery"
      />
      <div v-else key="main" ref="mainContent">
        <Navbar />
        <main>
          <HeroSection />
          <AboutSection />
          <SkillsSection @openGallery="openGallery" />
          <ContactSection />
          <GuestbookSection />
        </main>
        <Footer />
      </div>
    </Transition>
  </div>
</template>

<script setup>
import { ref, nextTick } from 'vue'
import ParticleBackground from './components/ParticleBackground.vue'
import Navbar from './components/Navbar.vue'
import HeroSection from './components/HeroSection.vue'
import AboutSection from './components/AboutSection.vue'
import SkillsSection from './components/SkillsSection.vue'
import ContactSection from './components/ContactSection.vue'
import GuestbookSection from './components/GuestbookSection.vue'
import Footer from './components/Footer.vue'
import GalleryPage from './components/GalleryPage.vue'

const currentGallery = ref(null)
const mainContent = ref(null)
const savedScrollPosition = ref(0)
const shouldScrollToSkills = ref(false)

const openGallery = (hobby) => {
  const skillsSection = document.getElementById('skills')
  if (skillsSection) {
    savedScrollPosition.value = skillsSection.offsetTop + 350
  } else {
    savedScrollPosition.value = window.scrollY
  }
  currentGallery.value = hobby
}

const closeGallery = () => {
  shouldScrollToSkills.value = true
  currentGallery.value = null
}

const onPageEnter = () => {
  if (shouldScrollToSkills.value) {
    shouldScrollToSkills.value = false
    nextTick(() => {
      setTimeout(() => {
        window.scrollTo({
          top: savedScrollPosition.value,
          behavior: 'smooth'
        })
      }, 50)
    })
  }
}
</script>

<style scoped>
.app {
  min-height: 100vh;
  position: relative;
}

main {
  position: relative;
  z-index: 1;
}

.page-fade-enter-active,
.page-fade-leave-active {
  transition: all 0.5s ease;
}

.page-fade-enter-from {
  opacity: 0;
  transform: scale(0.95);
}

.page-fade-leave-to {
  opacity: 0;
  transform: scale(1.05);
}
</style>
