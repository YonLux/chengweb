<template>
  <section id="home" class="hero">
    <div class="hero-content">
      <div class="hero-text">
        <p class="greeting animate-slideInLeft">你好，我是</p>
        <h1 class="name animate-slideInLeft delay-1">
          <span class="name-text">开发者</span>
          <span class="cursor">|</span>
        </h1>
        <div class="typing-container animate-slideInLeft delay-2">
          <span class="static-text">我是一名 </span>
          <span class="typing-text">{{ typedText }}</span>
        </div>
        <p class="intro animate-slideInLeft delay-3">
          热爱编程，专注于创造优雅的数字体验
        </p>
        <div class="hero-buttons animate-slideInLeft delay-4">
          <a href="#contact" class="btn btn-primary">
            <i class="fas fa-paper-plane"></i>
            联系我
          </a>
          <a href="#skills" class="btn btn-secondary">
            <i class="fas fa-code"></i>
            查看技能
          </a>
        </div>
      </div>
      <div class="hero-image animate-slideInRight delay-2">
        <div class="image-container">
          <div class="image-glow"></div>
          <div class="avatar-placeholder">
            <i class="fas fa-user-astronaut"></i>
          </div>
          <div class="floating-icons">
            <span class="float-icon icon-1"><i class="fab fa-python"></i></span>
            <span class="float-icon icon-2"><i class="fab fa-js-square"></i></span>
            <span class="float-icon icon-3"><i class="fab fa-vuejs"></i></span>
            <span class="float-icon icon-4"><i class="fab fa-html5"></i></span>
          </div>
        </div>
      </div>
    </div>
    <div class="scroll-indicator">
      <div class="mouse">
        <div class="wheel"></div>
      </div>
      <p>向下滚</p>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const roles = ['全栈开发者', 'Python工程师', '前端开发者', '技术爱好者']
const typedText = ref('')
const roleIndex = ref(0)
const charIndex = ref(0)
const isDeleting = ref(false)

const typeEffect = () => {
  const currentRole = roles[roleIndex.value]
  
  if (isDeleting.value) {
    typedText.value = currentRole.substring(0, charIndex.value - 1)
    charIndex.value--
  } else {
    typedText.value = currentRole.substring(0, charIndex.value + 1)
    charIndex.value++
  }

  if (!isDeleting.value && charIndex.value === currentRole.length) {
    setTimeout(() => {
      isDeleting.value = true
    }, 2000)
  } else if (isDeleting.value && charIndex.value === 0) {
    isDeleting.value = false
    roleIndex.value = (roleIndex.value + 1) % roles.length
  }
}

let typingInterval

onMounted(() => {
  typingInterval = setInterval(typeEffect, 100)
})

onUnmounted(() => {
  clearInterval(typingInterval)
})
</script>

<style scoped>
.hero {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 100px 20px;
  position: relative;
}

.hero-content {
  max-width: 1200px;
  width: 100%;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 60px;
  align-items: center;
}

.hero-text {
  opacity: 0;
}

.greeting {
  font-size: 1.2rem;
  color: var(--secondary-color);
  margin-bottom: 10px;
  opacity: 0;
}

.name {
  font-size: 4rem;
  font-weight: 700;
  margin-bottom: 20px;
  opacity: 0;
}

.name-text {
  background: linear-gradient(135deg, #fff 0%, #a29bfe 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.cursor {
  color: var(--primary-color);
  animation: blink 1s infinite;
}

@keyframes blink {
  0%, 50% { opacity: 1; }
  51%, 100% { opacity: 0; }
}

.typing-container {
  font-size: 1.5rem;
  margin-bottom: 20px;
  opacity: 0;
}

.static-text {
  color: var(--text-secondary);
}

.typing-text {
  color: var(--accent-color);
  font-weight: 600;
}

.intro {
  font-size: 1.1rem;
  color: var(--text-secondary);
  margin-bottom: 30px;
  max-width: 500px;
  opacity: 0;
}

.hero-buttons {
  display: flex;
  gap: 20px;
  opacity: 0;
}

.btn {
  padding: 15px 35px;
  border-radius: 50px;
  font-size: 1rem;
  font-weight: 600;
  text-decoration: none;
  display: inline-flex;
  align-items: center;
  gap: 10px;
  transition: all 0.3s ease;
  cursor: pointer;
}

.btn-primary {
  background: var(--gradient-1);
  color: white;
  border: none;
}

.btn-primary:hover {
  transform: translateY(-3px);
  box-shadow: 0 10px 30px rgba(108, 92, 231, 0.4);
}

.btn-secondary {
  background: transparent;
  color: var(--text-primary);
  border: 2px solid var(--primary-color);
}

.btn-secondary:hover {
  background: var(--primary-color);
  transform: translateY(-3px);
}

.hero-image {
  display: flex;
  justify-content: center;
  opacity: 0;
}

.image-container {
  position: relative;
  width: 400px;
  height: 400px;
}

.image-glow {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 300px;
  height: 300px;
  background: var(--gradient-1);
  border-radius: 50%;
  filter: blur(60px);
  opacity: 0.5;
  animation: pulse 4s ease-in-out infinite;
}

.avatar-placeholder {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 280px;
  height: 280px;
  background: linear-gradient(135deg, rgba(108, 92, 231, 0.3), rgba(162, 155, 254, 0.1));
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 3px solid rgba(108, 92, 231, 0.5);
  animation: float 6s ease-in-out infinite;
}

.avatar-placeholder i {
  font-size: 8rem;
  background: var(--gradient-1);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.floating-icons {
  position: absolute;
  width: 100%;
  height: 100%;
}

.float-icon {
  position: absolute;
  width: 60px;
  height: 60px;
  background: var(--bg-card);
  border-radius: 15px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.8rem;
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  animation: float 4s ease-in-out infinite;
}

.icon-1 {
  top: 10%;
  left: -10%;
  color: #3776ab;
  animation-delay: 0s;
}

.icon-2 {
  top: 10%;
  right: -10%;
  color: #f7df1e;
  animation-delay: 1s;
}

.icon-3 {
  bottom: 10%;
  left: -10%;
  color: #4fc08d;
  animation-delay: 2s;
}

.icon-4 {
  bottom: 10%;
  right: -10%;
  color: #e34f26;
  animation-delay: 3s;
}

.scroll-indicator {
  position: absolute;
  bottom: 30px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
  opacity: 0;
  animation: fadeIn 1s ease-out 2s forwards;
}

.mouse {
  width: 25px;
  height: 40px;
  border: 2px solid var(--text-secondary);
  border-radius: 15px;
  display: flex;
  justify-content: center;
  padding-top: 8px;
}

.wheel {
  width: 4px;
  height: 8px;
  background: var(--primary-color);
  border-radius: 2px;
  animation: scroll 2s infinite;
}

@keyframes scroll {
  0% { transform: translateY(0); opacity: 1; }
  100% { transform: translateY(10px); opacity: 0; }
}

.scroll-indicator p {
  font-size: 0.8rem;
  color: var(--text-secondary);
}

@media (max-width: 968px) {
  .hero-content {
    grid-template-columns: 1fr;
    text-align: center;
  }

  .name {
    font-size: 3rem;
  }

  .hero-buttons {
    justify-content: center;
  }

  .hero-image {
    order: -1;
  }

  .image-container {
    width: 300px;
    height: 300px;
  }

  .avatar-placeholder {
    width: 200px;
    height: 200px;
  }

  .avatar-placeholder i {
    font-size: 5rem;
  }

  .float-icon {
    width: 45px;
    height: 45px;
    font-size: 1.3rem;
  }
}

@media (max-width: 480px) {
  .name {
    font-size: 2.5rem;
  }

  .typing-container {
    font-size: 1.2rem;
  }

  .hero-buttons {
    flex-direction: column;
    align-items: center;
  }

  .btn {
    width: 100%;
    justify-content: center;
  }
}
</style>
