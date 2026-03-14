<template>
  <div class="particles">
    <div
      v-for="particle in particles"
      :key="particle.id"
      class="particle"
      :style="particle.style"
    ></div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const particles = ref([])

const createParticle = (id) => {
  const size = Math.random() * 5 + 2
  const left = Math.random() * 100
  const duration = Math.random() * 20 + 10
  const delay = Math.random() * 20
  const colors = ['#6c5ce7', '#a29bfe', '#fd79a8', '#00cec9', '#81ecec']
  const color = colors[Math.floor(Math.random() * colors.length)]

  return {
    id,
    style: {
      width: `${size}px`,
      height: `${size}px`,
      left: `${left}%`,
      bottom: '-10px',
      background: color,
      animationDuration: `${duration}s`,
      animationDelay: `${delay}s`,
      boxShadow: `0 0 ${size * 2}px ${color}`
    }
  }
}

onMounted(() => {
  for (let i = 0; i < 50; i++) {
    particles.value.push(createParticle(i))
  }
})
</script>

<style scoped>
.particles {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 0;
  overflow: hidden;
}

.particle {
  position: absolute;
  border-radius: 50%;
  animation: particleFloat linear infinite;
}

@keyframes particleFloat {
  0% {
    transform: translateY(0) translateX(0) rotate(0deg);
    opacity: 0;
  }
  10% {
    opacity: 1;
  }
  90% {
    opacity: 1;
  }
  100% {
    transform: translateY(-100vh) translateX(100px) rotate(720deg);
    opacity: 0;
  }
}
</style>
