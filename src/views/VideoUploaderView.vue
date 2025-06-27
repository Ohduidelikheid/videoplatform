<template>
  <div class="layout">
    <!-- Sidebar -->
    <aside class="sidebar">
      <nav class="nav-links">
        <RouterLink to="/">Главная</RouterLink>
        <RouterLink to="/about">О Нас</RouterLink>
        <RouterLink to="/contact">Контакты</RouterLink>
        <RouterLink to="/VideoU">Загрузка видео</RouterLink>
      </nav>
    </aside>

    <!-- Content -->
    <div class="main-content">
      <div class="top-bar">
        <button class="theme-toggle" @click="toggleTheme">
          Переключить тему
        </button>
      </div>

      <h1 class="title">Загрузка видеолекций</h1>

      <div class="upload-controls">
        <label class="file-label">
          Выбрать файл
          <input type="file" @change="handleFileChange" hidden />
        </label>

        <input
          type="text"
          class="input"
          v-model="videoTitle"
          placeholder="Введите название"
        />

        <button class="upload-button" @click="uploadVideo" :disabled="!selectedFile || !videoTitle">
          Загрузить
        </button>
      </div>

      <div class="upload-hint">
        <template v-if="selectedFile">
          Название загружаемого файла: <strong>{{ selectedFile.name }}</strong>
        </template>
        <template v-else>
          Поддерживаются форматы: MP4, WebM
        </template>
      </div>

      <h2 class="subtitle" v-if="videoUploaded">Загруженное видео</h2>
      <div class="video-preview" v-if="videoUploaded">
        <video class="video-player" controls>
          <source :src="videoURL" type="video/mp4" />
          Ваш браузер не поддерживает видео.
        </video>
        <p class="video-title">{{ videoTitle }}</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const selectedFile = ref(null)
const videoURL = ref('')
const videoUploaded = ref(false)
const videoTitle = ref('')

function handleFileChange(event) {
  selectedFile.value = event.target.files[0]
  videoUploaded.value = false
}

function uploadVideo() {
  if (!selectedFile.value || !videoTitle.value) return
  videoURL.value = URL.createObjectURL(selectedFile.value)
  videoUploaded.value = true
}

function toggleTheme() {
  if (typeof window !== 'undefined' && typeof window.toggleTheme === 'function') {
    window.toggleTheme()
  }
}
</script>

<style scoped>
.layout {
  display: flex;
  height: 100vh;
  width: 100%;
  overflow-x: hidden;
}

.sidebar {
  width: 240px;
  padding: 2rem 1rem;
  border-right: 1px solid #ccc;
  background-color: inherit;
}

.nav-links {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.nav-links a {
  text-decoration: none;
  color: inherit;
  font-weight: 500;
}

.main-content {
  flex-grow: 1;
  padding: 2rem 2rem 2rem 1rem;
  display: flex;
  flex-direction: column;
  background-color: inherit;
  overflow-y: auto;
}

@media (max-width: 1200px) {
  .main-content {
    max-width: 900px;
    margin-left: auto;
    margin-right: auto;
    padding-left: 1rem;
    padding-right: 1rem;
  }
}

.top-bar {
  display: flex;
  justify-content: flex-end;
  margin-bottom: 2rem;
}

.theme-toggle {
  padding: 0.5rem 1rem;
  border: 1px solid currentColor;
  background: none;
  border-radius: 20px;
  cursor: pointer;
  font-size: 0.9rem;
}

.title {
  font-size: 1.6rem;
  font-weight: 600;
  margin-bottom: 1.5rem;
}

.upload-controls {
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: 0.8rem;
  margin-bottom: 1rem;
  width: 100%;
  white-space: nowrap;
  overflow: hidden;
}

.upload-controls > * {
  min-width: 0;
}

.file-label {
  background-color: #007aff;
  color: white;
  padding: 0.5rem 1rem;
  border-radius: 8px;
  cursor: pointer;
  font-size: 0.9rem;
  flex-shrink: 0;
  white-space: nowrap;
}

.input {
  flex-grow: 1;
  padding: 0.5rem 1rem;
  border-radius: 8px;
  border: 1px solid #ccc;
  font-size: 0.9rem;
  min-width: 120px;
}

.upload-button {
  background-color: #28a745;
  color: white;
  padding: 0.5rem 1.2rem;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 0.9rem;
  flex-shrink: 0;
  white-space: nowrap;
}

.upload-button:disabled {
  background-color: #a5d6a7;
  cursor: not-allowed;
}

.upload-hint {
  font-size: 0.9rem;
  color: #777;
  margin-top: 0.5rem;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.subtitle {
  font-size: 1.2rem;
  font-weight: 600;
  margin-top: 2rem;
}

.video-preview {
  margin-top: 1rem;
  width: 100%;
}

.video-player {
  width: 100%;
  aspect-ratio: 16 / 9;
  border-radius: 12px;
  background: black;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}

.video-title {
  margin-top: 0.5rem;
}

/* Глобальный фикс от горизонтального скролла */
:deep(html), :deep(body) {
  overflow-x: hidden;
  margin: 0;
  padding: 0;
}
</style>
