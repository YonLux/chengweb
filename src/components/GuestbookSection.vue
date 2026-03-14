<template>
  <section id="guestbook" class="guestbook">
    <div class="container">
      <h2 class="section-title">留言板</h2>
      <div class="guestbook-content">
        <div class="message-form">
          <h3 class="form-title">写下您的留言</h3>
          <form @submit.prevent="addMessage">
            <div class="input-group">
              <input
                type="text"
                v-model="newMessage.name"
                placeholder="您的昵称"
                required
              />
              <i class="fas fa-user"></i>
            </div>
            <div class="input-group">
              <textarea
                v-model="newMessage.content"
                placeholder="写下您想说的话..."
                rows="4"
                required
              ></textarea>
              <i class="fas fa-pen"></i>
            </div>
            <div class="mood-selector">
              <span class="mood-label">选择心情：</span>
              <div class="mood-options">
                <button
                  type="button"
                  v-for="mood in moods"
                  :key="mood.emoji"
                  class="mood-btn"
                  :class="{ active: newMessage.mood === mood.emoji }"
                  @click="newMessage.mood = mood.emoji"
                >
                  {{ mood.emoji }}
                </button>
              </div>
            </div>
            <button type="submit" class="submit-btn">
              <i class="fas fa-paper-plane"></i>
              发表留言
            </button>
          </form>
        </div>
        <div class="messages-list">
          <h3 class="list-title">
            <i class="fas fa-comments"></i>
            留言墙 ({{ messages.length }})
          </h3>
          <div class="messages-container" ref="messagesContainer">
            <TransitionGroup name="message" tag="div" class="messages-wrapper">
              <div
                class="message-card"
                v-for="msg in messages"
                :key="msg.id"
              >
                <div class="message-header">
                  <div class="avatar">
                    {{ msg.name.charAt(0).toUpperCase() }}
                  </div>
                  <div class="message-info">
                    <span class="message-name">{{ msg.name }}</span>
                    <span class="message-time">{{ msg.time }}</span>
                  </div>
                  <span class="message-mood">{{ msg.mood }}</span>
                </div>
                <p class="message-content">{{ msg.content }}</p>
                <div class="message-actions">
                  <button class="action-btn" @click="likeMessage(msg)">
                    <i class="fas fa-heart"></i>
                    {{ msg.likes }}
                  </button>
                </div>
              </div>
            </TransitionGroup>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, reactive, onMounted } from 'vue'

const moods = [
  { emoji: '😊' },
  { emoji: '🎉' },
  { emoji: '💪' },
  { emoji: '❤️' },
  { emoji: '🌟' },
  { emoji: '🚀' }
]

const newMessage = reactive({
  name: '',
  content: '',
  mood: '😊'
})

const messages = ref([])

const addMessage = () => {
  const message = {
    id: Date.now(),
    name: newMessage.name,
    content: newMessage.content,
    mood: newMessage.mood,
    time: new Date().toLocaleString('zh-CN'),
    likes: 0
  }
  messages.value.unshift(message)
  saveMessages()
  newMessage.name = ''
  newMessage.content = ''
  newMessage.mood = '😊'
}

const likeMessage = (msg) => {
  msg.likes++
  saveMessages()
}

const saveMessages = () => {
  localStorage.setItem('guestbook_messages', JSON.stringify(messages.value))
}

const loadMessages = () => {
  const saved = localStorage.getItem('guestbook_messages')
  if (saved) {
    messages.value = JSON.parse(saved)
  } else {
    messages.value = [
      {
        id: 1,
        name: '访客小明',
        content: '网站做得非常棒！动画效果很炫酷，继续加油！',
        mood: '🎉',
        time: '2024/01/15 14:30',
        likes: 12
      },
      {
        id: 2,
        name: '技术爱好者',
        content: '从你的技术栈可以看出你是一个很有追求的开发者，期待更多作品！',
        mood: '💪',
        time: '2024/01/14 10:20',
        likes: 8
      },
      {
        id: 3,
        name: '路人甲',
        content: '路过打卡，页面设计很有创意！',
        mood: '🌟',
        time: '2024/01/13 16:45',
        likes: 5
      }
    ]
  }
}

onMounted(() => {
  loadMessages()
})
</script>

<style scoped>
.guestbook {
  background: linear-gradient(180deg, transparent 0%, rgba(108, 92, 231, 0.05) 50%, transparent 100%);
}

.guestbook-content {
  display: grid;
  grid-template-columns: 1fr 1.5fr;
  gap: 40px;
}

.message-form {
  background: var(--bg-card);
  border-radius: 20px;
  padding: 30px;
  border: 1px solid rgba(255, 255, 255, 0.05);
  height: fit-content;
  position: sticky;
  top: 100px;
}

.form-title {
  font-size: 1.3rem;
  margin-bottom: 25px;
  color: var(--text-primary);
}

.input-group {
  position: relative;
  margin-bottom: 20px;
}

.input-group input,
.input-group textarea {
  width: 100%;
  padding: 15px 20px;
  padding-left: 45px;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 12px;
  color: var(--text-primary);
  font-size: 1rem;
  font-family: inherit;
  transition: all 0.3s ease;
  resize: none;
}

.input-group input:focus,
.input-group textarea:focus {
  outline: none;
  border-color: var(--primary-color);
  box-shadow: 0 0 20px rgba(108, 92, 231, 0.2);
}

.input-group input::placeholder,
.input-group textarea::placeholder {
  color: var(--text-secondary);
}

.input-group i {
  position: absolute;
  left: 15px;
  top: 15px;
  color: var(--text-secondary);
}

.input-group textarea + i {
  top: 15px;
}

.mood-selector {
  margin-bottom: 20px;
}

.mood-label {
  display: block;
  margin-bottom: 10px;
  color: var(--text-secondary);
  font-size: 0.9rem;
}

.mood-options {
  display: flex;
  gap: 10px;
}

.mood-btn {
  width: 45px;
  height: 45px;
  border: 2px solid rgba(255, 255, 255, 0.1);
  border-radius: 12px;
  background: transparent;
  font-size: 1.3rem;
  cursor: pointer;
  transition: all 0.3s ease;
}

.mood-btn:hover {
  border-color: var(--primary-color);
  transform: scale(1.1);
}

.mood-btn.active {
  border-color: var(--primary-color);
  background: rgba(108, 92, 231, 0.2);
  transform: scale(1.1);
}

.submit-btn {
  width: 100%;
  padding: 15px;
  background: var(--gradient-1);
  border: none;
  border-radius: 12px;
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

.messages-list {
  background: var(--bg-card);
  border-radius: 20px;
  padding: 30px;
  border: 1px solid rgba(255, 255, 255, 0.05);
}

.list-title {
  font-size: 1.3rem;
  margin-bottom: 25px;
  color: var(--text-primary);
  display: flex;
  align-items: center;
  gap: 10px;
}

.list-title i {
  color: var(--primary-color);
}

.messages-container {
  max-height: 600px;
  overflow-y: auto;
  padding-right: 10px;
}

.messages-container::-webkit-scrollbar {
  width: 6px;
}

.messages-container::-webkit-scrollbar-thumb {
  background: var(--primary-color);
  border-radius: 3px;
}

.messages-wrapper {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.message-card {
  background: rgba(255, 255, 255, 0.03);
  border-radius: 15px;
  padding: 20px;
  border: 1px solid rgba(255, 255, 255, 0.05);
  transition: all 0.3s ease;
}

.message-card:hover {
  border-color: rgba(108, 92, 231, 0.3);
  transform: translateX(5px);
}

.message-header {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-bottom: 12px;
}

.avatar {
  width: 40px;
  height: 40px;
  background: var(--gradient-1);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 600;
  color: white;
}

.message-info {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.message-name {
  font-weight: 600;
  color: var(--text-primary);
}

.message-time {
  font-size: 0.8rem;
  color: var(--text-secondary);
}

.message-mood {
  font-size: 1.5rem;
}

.message-content {
  color: var(--text-secondary);
  line-height: 1.6;
  margin-bottom: 15px;
}

.message-actions {
  display: flex;
  justify-content: flex-end;
}

.action-btn {
  background: transparent;
  border: none;
  color: var(--text-secondary);
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 5px;
  padding: 5px 10px;
  border-radius: 20px;
  transition: all 0.3s ease;
}

.action-btn:hover {
  color: var(--accent-color);
  background: rgba(253, 121, 168, 0.1);
}

.action-btn:hover i {
  animation: pulse 0.5s ease;
}

.message-enter-active,
.message-leave-active {
  transition: all 0.5s ease;
}

.message-enter-from {
  opacity: 0;
  transform: translateY(-30px);
}

.message-leave-to {
  opacity: 0;
  transform: translateX(30px);
}

@media (max-width: 968px) {
  .guestbook-content {
    grid-template-columns: 1fr;
  }

  .message-form {
    position: static;
  }
}
</style>
