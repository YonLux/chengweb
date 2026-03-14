<template>
  <section id="skills" class="skills">
    <div class="container">
      <h2 class="section-title">技术栈</h2>
      <div class="skills-grid">
        <div
          class="skill-card"
          v-for="(skill, index) in skills"
          :key="skill.name"
          :style="{ animationDelay: `${index * 0.1}s` }"
        >
          <div class="skill-icon" :style="{ background: skill.gradient }">
            <i :class="skill.icon"></i>
          </div>
          <h3 class="skill-name">{{ skill.name }}</h3>
          <p class="skill-description">{{ skill.description }}</p>
          <div class="skill-level">
            <div class="level-bar">
              <div
                class="level-fill"
                :style="{ width: skill.level + '%', background: skill.gradient }"
              ></div>
            </div>
            <span class="level-text">{{ skill.level }}%</span>
          </div>
        </div>
      </div>
      <div class="tech-stack">
        <h3 class="tech-title">兴趣爱好 <span class="hint">点击查看相册</span></h3>
        <div class="tech-icons">
          <div
            class="tech-item clickable"
            v-for="(tech, index) in techStack"
            :key="tech.name"
            :style="{ animationDelay: `${index * 0.05}s` }"
            @click="$emit('openGallery', tech)"
          >
            <i :class="tech.icon" :style="{ color: tech.color }"></i>
            <span>{{ tech.name }}</span>
            <div class="click-ripple"></div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
defineEmits(['openGallery'])

const skills = [
  {
    name: 'Python',
    icon: 'fab fa-python',
    description: '数据分析、机器学习、后端开发',
    level: 90,
    gradient: 'linear-gradient(135deg, #3776ab, #ffd43b)'
  },
  {
    name: 'JavaScript',
    icon: 'fab fa-js-square',
    description: '前端交互、Node.js后端开发',
    level: 85,
    gradient: 'linear-gradient(135deg, #f7df1e, #f0db4f)'
  },
  {
    name: 'Vue.js',
    icon: 'fab fa-vuejs',
    description: '响应式前端框架、组件化开发',
    level: 30,
    gradient: 'linear-gradient(135deg, #4fc08d, #42b883)'
  },
  {
    name: 'HTML/CSS',
    icon: 'fab fa-html5',
    description: '语义化标记、响应式布局、动画效果',
    level: 60,
    gradient: 'linear-gradient(135deg, #e34f26, #f06529)'
  },
  {
    name: '数据库',
    icon: 'fas fa-database',
    description: 'MySQL、MongoDB、Redis数据存储',
    level: 0,
    gradient: 'linear-gradient(135deg, #00758f, #f29111)'
  },
  {
    name: 'Git',
    icon: 'fab fa-git-alt',
    description: '版本控制、团队协作、CI/CD',
    level: 85,
    gradient: 'linear-gradient(135deg, #f05032, #de4c36)'
  }
]

const techStack = [
  { id: 'photography', name: '摄影', icon: 'fas fa-camera-retro', color: '#8e44ad', description: '用镜头记录生活中的美好瞬间' },
  { id: 'basketball', name: '篮球', icon: 'fas fa-basketball-ball', color: '#e67e22', description: '挥洒汗水，享受运动的快乐' },
  { id: 'volleyball', name: '排球', icon: 'fas fa-volleyball-ball', color: '#3498db', description: '团队协作，共同进步' },
  { id: 'soccer', name: '足球', icon: 'fas fa-futbol', color: '#27ae60', description: '绿茵场上的激情与梦想' },
  { id: 'novel', name: '小说', icon: 'fas fa-book', color: '#9b59b6', description: '在文字的世界里遨游' },
  { id: 'bicycle', name: '自行车', icon: 'fas fa-bicycle', color: '#1abc9c', description: '骑行在路上的自由时光' },
  { id: 'pingpong', name: '乒乓球', icon: 'fas fa-table-tennis', color: '#e74c3c', description: '国球魅力，技巧与速度' },
  { id: 'running', name: '跑步', icon: 'fas fa-running', color: '#f39c12', description: '用脚步丈量世界' }
]
</script>

<style scoped>
.skills {
  background: linear-gradient(180deg, transparent 0%, rgba(0, 206, 201, 0.03) 50%, transparent 100%);
}

.skills-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 30px;
  margin-bottom: 60px;
}

.skill-card {
  background: var(--bg-card);
  border-radius: 20px;
  padding: 30px;
  border: 1px solid rgba(255, 255, 255, 0.05);
  transition: all 0.4s ease;
  opacity: 0;
  transform: translateY(30px);
  animation: slideInUp 0.6s ease-out forwards;
}

.skill-card:hover {
  transform: translateY(-10px);
  border-color: rgba(108, 92, 231, 0.3);
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.3);
}

.skill-icon {
  width: 70px;
  height: 70px;
  border-radius: 15px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 2rem;
  color: white;
  margin-bottom: 20px;
  transition: transform 0.3s ease;
}

.skill-card:hover .skill-icon {
  transform: scale(1.1) rotate(5deg);
}

.skill-name {
  font-size: 1.3rem;
  margin-bottom: 10px;
  color: var(--text-primary);
}

.skill-description {
  color: var(--text-secondary);
  font-size: 0.9rem;
  margin-bottom: 20px;
  line-height: 1.6;
}

.skill-level {
  display: flex;
  align-items: center;
  gap: 15px;
}

.level-bar {
  flex: 1;
  height: 8px;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 4px;
  overflow: hidden;
}

.level-fill {
  height: 100%;
  border-radius: 4px;
  transition: width 1s ease-out;
}

.level-text {
  font-size: 0.9rem;
  font-weight: 600;
  color: var(--text-secondary);
  min-width: 40px;
}

.tech-stack {
  text-align: center;
}

.tech-title {
  font-size: 1.5rem;
  margin-bottom: 30px;
  color: var(--text-primary);
}

.hint {
  font-size: 0.8rem;
  color: var(--text-secondary);
  font-weight: normal;
  opacity: 0.7;
}

.tech-icons {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
}

.tech-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
  padding: 20px 30px;
  background: var(--bg-card);
  border-radius: 15px;
  border: 1px solid rgba(255, 255, 255, 0.05);
  transition: all 0.3s ease;
  opacity: 0;
  transform: scale(0.8);
  animation: bounceIn 0.5s ease-out forwards;
  position: relative;
  overflow: hidden;
}

.tech-item.clickable {
  cursor: pointer;
}

.tech-item.clickable:hover {
  transform: translateY(-8px) scale(1.05);
  border-color: rgba(108, 92, 231, 0.5);
  box-shadow: 0 15px 40px rgba(0, 0, 0, 0.3);
}

.tech-item.clickable:active {
  transform: translateY(-5px) scale(1.02);
}

.tech-item i {
  font-size: 2rem;
  transition: all 0.3s ease;
}

.tech-item.clickable:hover i {
  transform: scale(1.3) rotate(10deg);
}

.tech-item span {
  font-size: 0.85rem;
  color: var(--text-secondary);
  transition: color 0.3s ease;
}

.tech-item.clickable:hover span {
  color: var(--text-primary);
}

.click-ripple {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 0;
  height: 0;
  background: rgba(255, 255, 255, 0.3);
  border-radius: 50%;
  transform: translate(-50%, -50%);
  transition: width 0.6s ease, height 0.6s ease;
}

.tech-item.clickable:active .click-ripple {
  width: 300px;
  height: 300px;
}

@media (max-width: 968px) {
  .skills-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 640px) {
  .skills-grid {
    grid-template-columns: 1fr;
  }

  .tech-item {
    padding: 15px 20px;
  }
}
</style>
