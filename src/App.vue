<script setup lang="ts">
import PomodoroTimer from './components/PomodoroTimer.vue';
import TaskManager from './components/TaskManager.vue';
import { ref, watch } from 'vue';

const isDarkMode = ref(false);

watch(isDarkMode, (newValue) => {
  if (newValue) {
    document.body.classList.add('dark');
  } else {
    document.body.classList.remove('dark');
  }
});

const toggleTheme = () => {
  isDarkMode.value = !isDarkMode.value;
};
</script>

<template>
  <div :class="{ 'dark-mode': isDarkMode }">
    <header>
      <button @click="toggleTheme" class="theme-toggle-btn">
        <i :class="isDarkMode ? 'fas fa-sun' : 'fas fa-moon'"></i>
      </button>
    </header>
    <main class="container">
      <PomodoroTimer class="" />
      <TaskManager class="" />
    </main>
  </div>
</template>

<style scoped>
.theme-toggle-btn {
  padding: 10px 20px;
  font-size: 1rem;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  background-color: #4b3f72;
  color: #fdf7fa;
  transition: background-color 0.3s;
}

.theme-toggle-btn:hover {
  background-color: #1f2041;
}

body.dark .theme-toggle-btn {
  color: #4b3f72;
  background-color: #eae151;
}

body.dark .theme-toggle-btn:hover {
  background-color: #b5b78d;
}

.container {
  display: flex;
  flex-direction: row;
  justify-content: space-evenly;
  align-items: flex-start;
  flex-wrap: wrap; /* Permite quebra de linha se a tela for muito pequena */
  padding: 20px;
}

@media (max-width: 1200px) {
  .container {
    flex-direction: column;
    align-items: center;
  }
}
</style>
