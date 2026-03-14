<template>
  <section id="websites" class="websites-section">
    <div class="container">
      <h2 class="section-title animate-fadeIn">
        <i class="fas fa-rocket"></i>
        网站导航
      </h2>
      
      <div class="websites-grid">
        <a
          v-for="(site, index) in websites"
          :key="site.id"
          :href="site.url"
          target="_blank"
          rel="noopener noreferrer"
          class="website-card"
          :style="{ animationDelay: `${index * 0.1}s` }"
        >
          <div class="card-glow"></div>
          <div class="card-border"></div>
          <div class="card-content">
            <div class="icon-wrapper">
              <div class="icon-ring"></div>
              <i :class="site.icon" :style="{ color: site.color }"></i>
            </div>
            <h3 class="site-name">{{ site.name }}</h3>
            <p class="site-desc">{{ site.description }}</p>
            <div class="hover-effect">
              <span>访问</span>
              <i class="fas fa-arrow-right"></i>
            </div>
          </div>
        </a>
      </div>
    </div>
    
    <div class="bg-effects">
      <div class="grid-lines"></div>
      <div class="floating-particles">
        <span v-for="n in 20" :key="n" class="particle"></span>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref } from 'vue'

const websites = ref([
  {
    id: 1,
    name: '刘野明的工具箱',
    icon: 'fas fa-book',
    url: 'https://tool.liumingye.cn/',
    description: '协作工具',
    color: '#ffffff'
  },
  {
    id: 2,
    name: 'Google',
    icon: 'fab fa-google',
    url: 'https://google.com',
    description: '搜索引擎',
    color: '#4285f4'
  },
  {
    id: 3,
    name: 'YouTube',
    icon: 'fab fa-youtube',
    url: 'https://youtube.com',
    description: '视频分享平台',
    color: '#ff0000'
  },
  {
    id: 4,
    name: 'Bilibili',
    icon: 'fab fa-bilibili',
    url: 'https://bilibili.com',
    description: '弹幕视频网站',
    color: '#00a1d6'
  },
  {
    id: 5,
    name: 'qwen',
    icon: 'fas fa-robot',
    url: 'https://www.qianwen.com/',
    description: 'AI 对话助手',
    color: '#1da1f2'
  },
  {
    id: 6,
    name: 'CSDN',
    icon: 'fas fa-blog',
    url: 'https://blog.csdn.net/2301_78367538?spm=1010.2135.3001.5343',
    description: '技术博客社区',
    color: '#fc5531'
  },
  {
    id: 7,
    name: 'ChatGPT',
    icon: 'fas fa-robot',
    url: 'https://chat.openai.com',
    description: 'AI 对话助手',
    color: '#10a37f'
  },
  {

    id: 8,
    name: 'GitHub',
    icon: 'fab fa-github',
    url: 'https://github.com',
    description: '代码托管平台',
    color: '#ffffff'

  }
])
</script>

<style scoped>
.websites-section {
  padding: 100px 0;
  position: relative;
  overflow: hidden;
  background: linear-gradient(180deg, transparent 0%, rgba(108, 92, 231, 0.05) 50%, transparent 100%);
}

.section-title {
  text-align: center;
  margin-bottom: 60px;
  font-size: 2.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 15px;
}

.section-title i {
  background: linear-gradient(135deg, #6c5ce7, #00cec9);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.websites-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 30px;
  position: relative;
  z-index: 2;
}

.website-card {
  position: relative;
  background: rgba(255, 255, 255, 0.03);
  border-radius: 20px;
  padding: 30px;
  text-decoration: none;
  color: inherit;
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  overflow: hidden;
  animation: slideInUp 0.6s ease-out forwards;
  opacity: 0;
}

.website-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(135deg, 
    rgba(108, 92, 231, 0.1) 0%, 
    rgba(0, 206, 201, 0.1) 100%);
  opacity: 0;
  transition: opacity 0.3s ease;
}

.website-card:hover::before {
  opacity: 1;
}

.card-glow {
  position: absolute;
  top: -50%;
  left: -50%;
  width: 200%;
  height: 200%;
  background: radial-gradient(circle, rgba(108, 92, 231, 0.15) 0%, transparent 70%);
  opacity: 0;
  transition: opacity 0.5s ease;
  pointer-events: none;
}

.website-card:hover .card-glow {
  opacity: 1;
}

.card-border {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  border-radius: 20px;
  padding: 2px;
  background: linear-gradient(135deg, 
    rgba(108, 92, 231, 0.5) 0%, 
    rgba(0, 206, 201, 0.5) 50%,
    rgba(108, 92, 231, 0.5) 100%);
  background-size: 200% 200%;
  -webkit-mask: 
    linear-gradient(#fff 0 0) content-box, 
    linear-gradient(#fff 0 0);
  -webkit-mask-composite: xor;
  mask-composite: exclude;
  opacity: 0.5;
  transition: all 0.5s ease;
  animation: borderGlow 3s ease infinite;
}

.website-card:hover .card-border {
  opacity: 1;
  background-size: 100% 100%;
}

@keyframes borderGlow {
  0%, 100% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
}

.card-content {
  position: relative;
  z-index: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
}

.icon-wrapper {
  position: relative;
  width: 80px;
  height: 80px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 20px;
}

.icon-wrapper i {
  font-size: 2.5rem;
  z-index: 2;
  transition: all 0.3s ease;
}

.icon-ring {
  position: absolute;
  width: 100%;
  height: 100%;
  border: 2px solid rgba(108, 92, 231, 0.3);
  border-radius: 50%;
  animation: ringPulse 2s ease-in-out infinite;
}

.website-card:hover .icon-ring {
  border-color: rgba(0, 206, 201, 0.5);
  animation: ringRotate 1s linear infinite;
}

@keyframes ringPulse {
  0%, 100% {
    transform: scale(1);
    opacity: 0.5;
  }
  50% {
    transform: scale(1.1);
    opacity: 1;
  }
}

@keyframes ringRotate {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

.site-name {
  font-size: 1.3rem;
  font-weight: 600;
  margin-bottom: 8px;
  background: linear-gradient(135deg, #ffffff 0%, #b8b8d1 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.site-desc {
  font-size: 0.9rem;
  color: var(--text-secondary);
  margin-bottom: 15px;
}

.hover-effect {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 8px 20px;
  background: linear-gradient(135deg, rgba(108, 92, 231, 0.2), rgba(0, 206, 201, 0.2));
  border-radius: 20px;
  font-size: 0.85rem;
  color: var(--text-primary);
  opacity: 0;
  transform: translateY(10px);
  transition: all 0.3s ease;
}

.website-card:hover .hover-effect {
  opacity: 1;
  transform: translateY(0);
}

.website-card:hover .hover-effect i {
  animation: arrowMove 0.5s ease infinite;
}

@keyframes arrowMove {
  0%, 100% {
    transform: translateX(0);
  }
  50% {
    transform: translateX(5px);
  }
}

.website-card:hover {
  transform: translateY(-10px) scale(1.02);
}

.website-card:hover .icon-wrapper i {
  transform: scale(1.2);
}

.bg-effects {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  pointer-events: none;
  overflow: hidden;
}

.grid-lines {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-image: 
    linear-gradient(rgba(108, 92, 231, 0.03) 1px, transparent 1px),
    linear-gradient(90deg, rgba(108, 92, 231, 0.03) 1px, transparent 1px);
  background-size: 50px 50px;
  animation: gridMove 20s linear infinite;
}

@keyframes gridMove {
  from {
    background-position: 0 0;
  }
  to {
    background-position: 50px 50px;
  }
}

.floating-particles {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}

.particle {
  position: absolute;
  width: 4px;
  height: 4px;
  background: rgba(0, 206, 201, 0.5);
  border-radius: 50%;
  animation: floatParticle 15s linear infinite;
}

.particle:nth-child(1) { left: 10%; animation-delay: 0s; }
.particle:nth-child(2) { left: 20%; animation-delay: 2s; }
.particle:nth-child(3) { left: 30%; animation-delay: 4s; }
.particle:nth-child(4) { left: 40%; animation-delay: 1s; }
.particle:nth-child(5) { left: 50%; animation-delay: 3s; }
.particle:nth-child(6) { left: 60%; animation-delay: 5s; }
.particle:nth-child(7) { left: 70%; animation-delay: 2.5s; }
.particle:nth-child(8) { left: 80%; animation-delay: 4.5s; }
.particle:nth-child(9) { left: 90%; animation-delay: 1.5s; }
.particle:nth-child(10) { left: 5%; animation-delay: 3.5s; }
.particle:nth-child(11) { left: 15%; animation-delay: 0.5s; }
.particle:nth-child(12) { left: 25%; animation-delay: 2.5s; }
.particle:nth-child(13) { left: 35%; animation-delay: 4.5s; }
.particle:nth-child(14) { left: 45%; animation-delay: 1.5s; }
.particle:nth-child(15) { left: 55%; animation-delay: 3.5s; }
.particle:nth-child(16) { left: 65%; animation-delay: 5.5s; }
.particle:nth-child(17) { left: 75%; animation-delay: 0.8s; }
.particle:nth-child(18) { left: 85%; animation-delay: 2.8s; }
.particle:nth-child(19) { left: 95%; animation-delay: 4.8s; }
.particle:nth-child(20) { left: 50%; animation-delay: 1.8s; }

@keyframes floatParticle {
  0% {
    top: 100%;
    opacity: 0;
  }
  10% {
    opacity: 1;
  }
  90% {
    opacity: 1;
  }
  100% {
    top: -10%;
    opacity: 0;
  }
}

@media (max-width: 768px) {
  .websites-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 15px;
  }
  
  .website-card {
    padding: 20px;
  }
  
  .icon-wrapper {
    width: 60px;
    height: 60px;
  }
  
  .icon-wrapper i {
    font-size: 2rem;
  }
  
  .site-name {
    font-size: 1rem;
  }
  
  .site-desc {
    font-size: 0.8rem;
  }
}

@media (max-width: 480px) {
  .websites-grid {
    grid-template-columns: 1fr;
  }
}
</style>
