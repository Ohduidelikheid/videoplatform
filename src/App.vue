<template>
  <div :class="theme">
    <RouterView />
  </div>
</template>

<script setup>
import { ref, onMounted, watchEffect } from 'vue'

const theme = ref(localStorage.getItem('theme') || getSystemTheme())

function getSystemTheme() {
  return window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light'
}

function applyTheme() {
  document.documentElement.className = theme.value
  localStorage.setItem('theme', theme.value)
}

onMounted(() => {
  applyTheme()
  window.toggleTheme = () => {
    theme.value = theme.value === 'dark' ? 'light' : 'dark'
    applyTheme()
  }
})

watchEffect(() => {
  applyTheme()
})
</script>

<style>
html.light {
  background-color: #ffffff;
  color: #1a1a1a;
}

html.dark {
  background-color: #111111;
  color: #f1f1f1;
}

body {
  margin: 0;
  padding: 0;
}
</style>
