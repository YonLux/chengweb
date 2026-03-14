<template>
  <section id="guestbook" class="guestbook">
    <div class="container">
      <h2 class="section-title">
        <i class="fas fa-comments"></i>
        留言板
      </h2>
      <div class="guestbook-wrapper">
        <div class="giscus-container">
          <div class="giscus"></div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { onMounted } from 'vue'

const GISCUS_CONFIG = {
  repo: 'YonLux/chengweb',
  repoId: 'R_kgDORmxMpQ',
  category: 'Announcements',
  categoryId: 'DIC_kwDORmxMpc4C4YIn',
  mapping: 'pathname',
  term: '',
  strict: '0',
  reactionsEnabled: '1',
  emitMetadata: '0',
  inputPosition: 'bottom',
  theme: 'preferred_color_scheme',
  lang: 'zh-CN',
  loading: 'lazy'
}

onMounted(() => {
  const script = document.createElement('script')
  script.src = 'https://giscus.app/client.js'
  script.setAttribute('data-repo', GISCUS_CONFIG.repo)
  script.setAttribute('data-repo-id', GISCUS_CONFIG.repoId)
  script.setAttribute('data-category', GISCUS_CONFIG.category)
  script.setAttribute('data-category-id', GISCUS_CONFIG.categoryId)
  script.setAttribute('data-mapping', GISCUS_CONFIG.mapping)
  script.setAttribute('data-strict', GISCUS_CONFIG.strict)
  script.setAttribute('data-reactions-enabled', GISCUS_CONFIG.reactionsEnabled)
  script.setAttribute('data-emit-metadata', GISCUS_CONFIG.emitMetadata)
  script.setAttribute('data-input-position', GISCUS_CONFIG.inputPosition)
  script.setAttribute('data-theme', GISCUS_CONFIG.theme)
  script.setAttribute('data-lang', GISCUS_CONFIG.lang)
  script.setAttribute('data-loading', GISCUS_CONFIG.loading)
  script.crossOrigin = 'anonymous'
  script.async = true
  
  const giscusContainer = document.querySelector('.giscus')
  if (giscusContainer) {
    giscusContainer.appendChild(script)
  }
})
</script>

<style scoped>
.guestbook {
  padding: 100px 0;
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
  background: linear-gradient(135deg, #6c5ce7, #a29bfe);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.guestbook-wrapper {
  max-width: 900px;
  margin: 0 auto;
}

.giscus-container {
  background: rgba(255, 255, 255, 0.03);
  border-radius: 20px;
  padding: 30px;
  border: 1px solid rgba(255, 255, 255, 0.05);
  min-height: 300px;
}

:deep(.giscus) {
  max-width: 100%;
}

:deep(.giscus-frame) {
  width: 100%;
  border: none;
  border-radius: 12px;
}

@media (max-width: 768px) {
  .giscus-container {
    padding: 20px;
  }
}
</style>
