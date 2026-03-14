<template>
  <footer class="footer">
    <div class="footer-content">
      <div class="footer-main">
        <div class="footer-brand">
          <a href="#" class="footer-logo">
            <span class="logo-text">Portfolio</span>
            <span class="logo-dot"></span>
          </a>
          <p class="footer-description">
            do more get less
            但行好事 莫问前程 
          </p>
        </div>
        <div class="footer-links">
          <div class="link-group">
            <h4>快速导航</h4>
            <ul>
              <li><a href="#home">首页</a></li>
              <li><a href="#about">关于</a></li>
              <li><a href="#skills">技能</a></li>
              <li><a href="#contact">联系</a></li>
            </ul>
          </div>
          <div class="link-group">
            <h4>社交媒体</h4>
            <div class="social-links">
              <a href="https://github.com/YonLux" target="_blank" class="social-link" title="GitHub">
                <i class="fab fa-github"></i>
              </a>
              <a href="#" class="social-link" title="LinkedIn">
                <i class="fab fa-linkedin-in"></i>
              </a>
              <a href="#" class="social-link" title="Twitter">
                <i class="fab fa-twitter"></i>
              </a>
              <a href="#" class="social-link" title="微信">
                <i class="fab fa-weixin"></i>
              </a>
            </div>
          </div>
        </div>
      </div>
      <div class="footer-bottom">
        <p class="copyright">
          © {{ currentYear }} Portfolio. All rights reserved.
        </p>
        <p class="tech-info">
          Built with <i class="fas fa-heart"></i> using Vue.js & Vite
        </p>
      </div>
    </div>
    <button
      class="back-to-top"
      :class="{ visible: showBackToTop }"
      @click="scrollToTop"
    >
      <i class="fas fa-arrow-up"></i>
    </button>
  </footer>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'

const currentYear = computed(() => new Date().getFullYear())
const showBackToTop = ref(false)

const handleScroll = () => {
  showBackToTop.value = window.scrollY > 500
}

const scrollToTop = () => {
  window.scrollTo({
    top: 0,
    behavior: 'smooth'
  })
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll)
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})
</script>

<style scoped>
.footer {
  background: linear-gradient(180deg, transparent 0%, rgba(12, 12, 29, 0.8) 20%);
  padding: 60px 20px 30px;
  position: relative;
}

.footer-content {
  max-width: 1200px;
  margin: 0 auto;
}

.footer-main {
  display: grid;
  grid-template-columns: 1.5fr 1fr;
  gap: 60px;
  padding-bottom: 40px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.footer-logo {
  font-size: 1.8rem;
  font-weight: 700;
  text-decoration: none;
  display: inline-flex;
  align-items: center;
  gap: 2px;
  margin-bottom: 15px;
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

.footer-description {
  color: var(--text-secondary);
  line-height: 1.8;
  max-width: 400px;
}

.footer-links {
  display: flex;
  gap: 60px;
}

.link-group h4 {
  color: var(--text-primary);
  margin-bottom: 20px;
  font-size: 1.1rem;
}

.link-group ul {
  list-style: none;
}

.link-group li {
  margin-bottom: 12px;
}

.link-group a {
  color: var(--text-secondary);
  text-decoration: none;
  transition: all 0.3s ease;
  display: inline-block;
}

.link-group a:hover {
  color: var(--primary-color);
  transform: translateX(5px);
}

.social-links {
  display: flex;
  gap: 15px;
}

.social-link {
  width: 45px;
  height: 45px;
  background: var(--bg-card);
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--text-secondary);
  font-size: 1.2rem;
  transition: all 0.3s ease;
  border: 1px solid rgba(255, 255, 255, 0.05);
}

.social-link:hover {
  background: var(--primary-color);
  color: white;
  transform: translateY(-5px);
  box-shadow: 0 10px 20px rgba(108, 92, 231, 0.3);
}

.footer-bottom {
  padding-top: 30px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.copyright {
  color: var(--text-secondary);
  font-size: 0.9rem;
}

.tech-info {
  color: var(--text-secondary);
  font-size: 0.9rem;
}

.tech-info i {
  color: var(--accent-color);
  animation: pulse 1.5s ease-in-out infinite;
}

.back-to-top {
  position: fixed;
  bottom: 30px;
  right: 30px;
  width: 50px;
  height: 50px;
  background: var(--gradient-1);
  border: none;
  border-radius: 50%;
  color: white;
  font-size: 1.2rem;
  cursor: pointer;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
  z-index: 1000;
  box-shadow: 0 5px 20px rgba(108, 92, 231, 0.4);
}

.back-to-top.visible {
  opacity: 1;
  visibility: visible;
}

.back-to-top:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 30px rgba(108, 92, 231, 0.6);
}

@media (max-width: 768px) {
  .footer-main {
    grid-template-columns: 1fr;
    gap: 40px;
  }

  .footer-links {
    gap: 40px;
  }

  .footer-bottom {
    flex-direction: column;
    gap: 10px;
    text-align: center;
  }
}
</style>
