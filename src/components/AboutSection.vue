<template>
  <section id="about" class="about">
    <div class="container">
      <h2 class="section-title">关于騁</h2>
      <div class="about-content">
        <div class="about-image" ref="imageRef">
          <div class="image-frame">
            <div class="image-placeholder">
              <i class="fas fa-laptop-code"></i>
            </div>
            <div class="frame-decoration"></div>
          </div>
        </div>
        <div class="about-text" ref="textRef">
          <h3 class="about-subtitle">
            <span class="highlight">热爱兴趣</span> 的开发者
          </h3>
          <p class="about-description">
            熟练python,c/c++,javascript,java,go,rust多种语言的helloworld打印
            掌握stm32，esp32等单片机开发以及使用ida对硬件进行逆向。画过四层高速pcb板。
          </p>
          <p class="about-description">
            希望为以后的边缘计算贡献一份小小的力量
          </p>
          <div class="about-stats">
            <div class="stat-item" v-for="stat in stats" :key="stat.label">
              <span class="stat-number">{{ stat.value }}</span>
              <span class="stat-label">{{ stat.label }}</span>
            </div>
          </div>
          <div class="about-tags">
            <span class="tag" v-for="tag in tags" :key="tag">{{ tag }}</span>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref } from 'vue'

const stats = [
  { value: '3+', label: '年经验' },
  { value: '50+', label: '项目完成' },
  { value: '100%', label: '热情投入' }
]

const tags = ['Python', 'JavaScript', 'c/c++', 'rust']
</script>

<style scoped>
.about {
  background: linear-gradient(180deg, transparent 0%, rgba(108, 92, 231, 0.05) 50%, transparent 100%);
  position: relative;
}

.about-content {
  display: grid;
  grid-template-columns: 1fr 1.5fr;
  gap: 80px;
  align-items: center;
}

.about-image {
  position: relative;
}

.image-frame {
  position: relative;
  width: 100%;
  aspect-ratio: 1;
  max-width: 400px;
}

.image-placeholder {
  width: 100%;
  height: 100%;
  background: linear-gradient(135deg, rgba(108, 92, 231, 0.2), rgba(162, 155, 254, 0.1));
  border-radius: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 2px solid rgba(108, 92, 231, 0.3);
  position: relative;
  z-index: 2;
}

.image-placeholder i {
  font-size: 6rem;
  background: var(--gradient-1);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.frame-decoration {
  position: absolute;
  top: 20px;
  left: 20px;
  width: 100%;
  height: 100%;
  border: 2px solid var(--primary-color);
  border-radius: 20px;
  z-index: 1;
  transition: all 0.3s ease;
}

.image-frame:hover .frame-decoration {
  top: 30px;
  left: 30px;
}

.about-text {
  opacity: 0;
  transform: translateX(50px);
  animation: slideInRight 0.8s ease-out forwards;
}

.about-subtitle {
  font-size: 2rem;
  margin-bottom: 20px;
  color: var(--text-primary);
}

.highlight {
  background: var(--gradient-1);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.about-description {
  color: var(--text-secondary);
  margin-bottom: 15px;
  line-height: 1.8;
}

.about-stats {
  display: flex;
  gap: 40px;
  margin: 30px 0;
}

.stat-item {
  text-align: center;
}

.stat-number {
  display: block;
  font-size: 2.5rem;
  font-weight: 700;
  background: var(--gradient-1);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.stat-label {
  color: var(--text-secondary);
  font-size: 0.9rem;
}

.about-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  margin-top: 20px;
}

.tag {
  padding: 8px 20px;
  background: var(--bg-card);
  border: 1px solid rgba(108, 92, 231, 0.3);
  border-radius: 25px;
  font-size: 0.9rem;
  color: var(--text-secondary);
  transition: all 0.3s ease;
}

.tag:hover {
  background: var(--primary-color);
  color: white;
  transform: translateY(-3px);
}

@media (max-width: 968px) {
  .about-content {
    grid-template-columns: 1fr;
    gap: 40px;
  }

  .about-image {
    display: flex;
    justify-content: center;
  }

  .image-frame {
    max-width: 300px;
  }

  .about-stats {
    justify-content: center;
  }

  .about-tags {
    justify-content: center;
  }
}
</style>
