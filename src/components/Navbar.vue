<template>
  <nav class="navbar" :class="{ scrolled: isScrolled }">
    <div class="nav-container">
      <a href="#" class="logo">
        <span class="logo-text">Portfolio</span>
        <span class="logo-dot"></span>
      </a>
      <ul class="nav-links">
        <li v-for="link in navLinks" :key="link.href">
          <a :href="link.href" class="nav-link">{{ link.text }}</a>
        </li>
      </ul>
      <button class="menu-toggle" @click="toggleMenu">
        <i :class="isMenuOpen ? 'fas fa-times' : 'fas fa-bars'"></i>
      </button>
    </div>
    <ul class="mobile-menu" :class="{ open: isMenuOpen }">
      <li v-for="link in navLinks" :key="link.href">
        <a :href="link.href" class="mobile-link" @click="closeMenu">{{ link.text }}</a>
      </li>
    </ul>
  </nav>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const isScrolled = ref(false)
const isMenuOpen = ref(false)

const navLinks = [
  { href: '#home', text: '首页' },
  { href: '#about', text: '关于' },
  { href: '#skills', text: '技能' },
  { href: '#contact', text: '联系' },
  { href: '#guestbook', text: '留言' }
]

const handleScroll = () => {
  isScrolled.value = window.scrollY > 50
}

const toggleMenu = () => {
  isMenuOpen.value = !isMenuOpen.value
}

const closeMenu = () => {
  isMenuOpen.value = false
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll)
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})
</script>

<style scoped>
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  padding: 20px 0;
  z-index: 1000;
  transition: all 0.3s ease;
}

.navbar.scrolled {
  background: rgba(12, 12, 29, 0.95);
  backdrop-filter: blur(20px);
  padding: 15px 0;
  box-shadow: 0 5px 30px rgba(0, 0, 0, 0.3);
}

.nav-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.logo {
  font-size: 1.8rem;
  font-weight: 700;
  color: var(--text-primary);
  text-decoration: none;
  display: flex;
  align-items: center;
  gap: 2px;
}

.logo-text {
  background: var(--gradient-1);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.logo-dot {
  width: 10px;
  height: 10px;
  background: var(--accent-color);
  border-radius: 50%;
  animation: pulse 2s ease-in-out infinite;
}

.nav-links {
  display: flex;
  list-style: none;
  gap: 40px;
}

.nav-link {
  color: var(--text-secondary);
  text-decoration: none;
  font-weight: 500;
  position: relative;
  transition: color 0.3s ease;
}

.nav-link::after {
  content: '';
  position: absolute;
  bottom: -5px;
  left: 0;
  width: 0;
  height: 2px;
  background: var(--gradient-1);
  transition: width 0.3s ease;
}

.nav-link:hover {
  color: var(--text-primary);
}

.nav-link:hover::after {
  width: 100%;
}

.menu-toggle {
  display: none;
  background: none;
  border: none;
  color: var(--text-primary);
  font-size: 1.5rem;
  cursor: pointer;
}

.mobile-menu {
  display: none;
  list-style: none;
  flex-direction: column;
  align-items: center;
  gap: 20px;
  padding: 20px;
  background: rgba(12, 12, 29, 0.98);
  position: absolute;
  top: 100%;
  left: 0;
  width: 100%;
  transform: translateY(-20px);
  opacity: 0;
  pointer-events: none;
  transition: all 0.3s ease;
}

.mobile-menu.open {
  transform: translateY(0);
  opacity: 1;
  pointer-events: all;
}

.mobile-link {
  color: var(--text-secondary);
  text-decoration: none;
  font-size: 1.1rem;
  transition: color 0.3s ease;
}

.mobile-link:hover {
  color: var(--text-primary);
}

@media (max-width: 768px) {
  .nav-links {
    display: none;
  }

  .menu-toggle {
    display: block;
  }

  .mobile-menu {
    display: flex;
  }
}
</style>
