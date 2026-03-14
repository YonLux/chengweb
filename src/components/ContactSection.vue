<template>
  <section id="contact" class="contact">
    <div class="container">
      <h2 class="section-title">联系方式</h2>
      <div class="contact-content">
        <div class="contact-info">
          <h3 class="contact-subtitle">随时联系我</h3>
          <p class="contact-description">
            如果您有任何合作意向，欢迎通过以下方式与我联系。
          </p>
          <div class="contact-cards">
            <div
              class="contact-card"
              v-for="(item, index) in contactItems"
              :key="item.type"
              :style="{ animationDelay: `${index * 0.1}s` }"
              @click="handleContact(item)"
            >
              <div class="card-icon" :style="{ background: item.gradient }">
                <i :class="item.icon"></i>
              </div>
              <div class="card-content">
                <h4>{{ item.type }}</h4>
                <p>{{ item.value }}</p>
              </div>
              <div class="card-action">
                <i class="fas fa-arrow-right"></i>
              </div>
            </div>
          </div>
        </div>
        <div class="contact-form-wrapper">
          <form class="contact-form" @submit.prevent="submitForm">
            <h3 class="form-title">发送消息</h3>
            <div class="form-group">
              <input
                type="text"
                v-model="form.name"
                placeholder="您的姓名"
                required
              />
              <i class="fas fa-user"></i>
            </div>
            <div class="form-group">
              <input
                type="email"
                v-model="form.email"
                placeholder="您的邮箱"
                required
              />
              <i class="fas fa-envelope"></i>
            </div>
            <div class="form-group">
              <textarea
                v-model="form.message"
                placeholder="您的消息..."
                rows="5"
                required
              ></textarea>
              <i class="fas fa-comment-alt"></i>
            </div>
            <button type="submit" class="submit-btn">
              <span>发送消息</span>
              <i class="fas fa-paper-plane"></i>
            </button>
          </form>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, reactive } from 'vue'

const contactItems = [
  {
    type: '邮箱',
    value: 'zcchangeman@email.com',
    icon: 'fas fa-envelope',
    gradient: 'linear-gradient(135deg, #6c5ce7, #a29bfe)',
    action: 'mailto:example@email.com'
  },
  {
    type: '微信',
    value: 'mylighters',
    icon: 'fab fa-weixin',
    gradient: 'linear-gradient(135deg, #07c160, #2aae67)',
    action: 'copy'
  },
  {
    type: '电话',
    value: '+86 1738108****',
    icon: 'fas fa-phone-alt',
    gradient: 'linear-gradient(135deg, #fd79a8, #e84393)',
    action: 'tel:+86138xxxxxxxx'
  },
  {
    type: 'QQ',
    value: '1846263491',
    icon: 'fab fa-qq',
    gradient: 'linear-gradient(135deg, #12b7f5, #1da1f2)',
    action: 'copy'
  }
]

const form = reactive({
  name: '',
  email: '',
  message: ''
})

const handleContact = (item) => {
  if (item.action === 'copy') {
    navigator.clipboard.writeText(item.value)
    alert(`已复制: ${item.value}`)
  } else if (item.action.startsWith('mailto:')) {
    window.location.href = item.action
  } else if (item.action.startsWith('tel:')) {
    window.location.href = item.action
  }
}

const submitForm = () => {
  alert(`感谢您的消息！\n姓名: ${form.name}\n邮箱: ${form.email}\n消息: ${form.message}`)
  form.name = ''
  form.email = ''
  form.message = ''
}
</script>

<style scoped>
.contact {
  background: linear-gradient(180deg, transparent 0%, rgba(253, 121, 168, 0.03) 50%, transparent 100%);
}

.contact-content {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 60px;
  align-items: start;
}

.contact-subtitle {
  font-size: 1.8rem;
  margin-bottom: 15px;
  color: var(--text-primary);
}

.contact-description {
  color: var(--text-secondary);
  margin-bottom: 30px;
  line-height: 1.8;
}

.contact-cards {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.contact-card {
  display: flex;
  align-items: center;
  gap: 20px;
  padding: 20px;
  background: var(--bg-card);
  border-radius: 15px;
  border: 1px solid rgba(255, 255, 255, 0.05);
  cursor: pointer;
  transition: all 0.3s ease;
  opacity: 0;
  transform: translateX(-30px);
  animation: slideInLeft 0.5s ease-out forwards;
}

.contact-card:hover {
  transform: translateX(10px);
  border-color: rgba(108, 92, 231, 0.3);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
}

.card-icon {
  width: 50px;
  height: 50px;
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.3rem;
  color: white;
  flex-shrink: 0;
}

.card-content {
  flex: 1;
}

.card-content h4 {
  font-size: 0.9rem;
  color: var(--text-secondary);
  margin-bottom: 5px;
}

.card-content p {
  font-size: 1rem;
  color: var(--text-primary);
  font-weight: 500;
}

.card-action {
  color: var(--text-secondary);
  transition: transform 0.3s ease;
}

.contact-card:hover .card-action {
  transform: translateX(5px);
  color: var(--primary-color);
}

.contact-form-wrapper {
  background: var(--bg-card);
  border-radius: 20px;
  padding: 40px;
  border: 1px solid rgba(255, 255, 255, 0.05);
}

.form-title {
  font-size: 1.5rem;
  margin-bottom: 30px;
  color: var(--text-primary);
}

.form-group {
  position: relative;
  margin-bottom: 20px;
}

.form-group input,
.form-group textarea {
  width: 100%;
  padding: 15px 20px;
  padding-left: 50px;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 10px;
  color: var(--text-primary);
  font-size: 1rem;
  font-family: inherit;
  transition: all 0.3s ease;
}

.form-group input:focus,
.form-group textarea:focus {
  outline: none;
  border-color: var(--primary-color);
  box-shadow: 0 0 20px rgba(108, 92, 231, 0.2);
}

.form-group input::placeholder,
.form-group textarea::placeholder {
  color: var(--text-secondary);
}

.form-group i {
  position: absolute;
  left: 20px;
  top: 50%;
  transform: translateY(-50%);
  color: var(--text-secondary);
  transition: color 0.3s ease;
}

.form-group textarea + i {
  top: 20px;
  transform: none;
}

.form-group input:focus + i,
.form-group textarea:focus + i {
  color: var(--primary-color);
}

.submit-btn {
  width: 100%;
  padding: 15px 30px;
  background: var(--gradient-1);
  border: none;
  border-radius: 10px;
  color: white;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  transition: all 0.3s ease;
}

.submit-btn:hover {
  transform: translateY(-3px);
  box-shadow: 0 10px 30px rgba(108, 92, 231, 0.4);
}

@media (max-width: 968px) {
  .contact-content {
    grid-template-columns: 1fr;
  }

  .contact-form-wrapper {
    padding: 30px;
  }
}
</style>
