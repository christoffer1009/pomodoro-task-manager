<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted } from 'vue';

const pomodoroDuration = ref(25);
const breakDuration = ref(5);
const remainingTime = ref(pomodoroDuration.value * 60);
const isRunning = ref(false);
const isPomodoro = ref(true);
let timer: NodeJS.Timeout | null = null;
const status = computed(() => (isPomodoro.value ? 'Pomodoro' : 'Break'));
const statusClass = computed(() => (isPomodoro.value ? 'pomodoro' : 'break'));

const time = computed(() => {
  const minutes = Math.floor(remainingTime.value / 60);
  const seconds = remainingTime.value % 60;
  return `${String(minutes).padStart(2, '0')}:${String(seconds).padStart(
    2,
    '0'
  )}`;
});

const startTimer = () => {
  if (isRunning.value) return;
  isRunning.value = true;
  timer = setInterval(() => {
    if (remainingTime.value <= 0) {
      stopTimer();
      if (isPomodoro.value) {
        alert('Pomodoro session ended. Time for a break!');
        remainingTime.value = breakDuration.value * 60;
        isPomodoro.value = false;
      } else {
        alert('Break ended. Time for a new Pomodoro session!');
        remainingTime.value = pomodoroDuration.value * 60;
        isPomodoro.value = true;
      }
      startTimer();
      return;
    }
    remainingTime.value--;
  }, 1000);
};

const stopTimer = () => {
  if (timer) {
    clearInterval(timer);
    timer = null;
  }
  isRunning.value = false;
};

const applySettings = () => {
  if (!isRunning.value) {
    remainingTime.value = isPomodoro.value
      ? pomodoroDuration.value * 60
      : breakDuration.value * 60;
  }
};
onMounted(() => {
  remainingTime.value = pomodoroDuration.value * 60;
});

onUnmounted(() => {
  if (timer) {
    clearInterval(timer);
  }
});
</script>

<template>
  <div class="pomodoro-card">
    <h1 class="title">Pomodoro Timer</h1>
    <div class="status]">{{ status }}</div>
    <div class="timer">{{ time }}</div>
    <div class="buttons">
      <button @click="startTimer" class="btn">
        <i class="fa fa-play" aria-hidden="true"></i>
      </button>
      <button @click="stopTimer" class="btn stop">
        <i class="fa fa-pause"></i>
      </button>
    </div>

    <div class="settings">
      <label>
        Pomodoro Duration (minutes):
        <input type="number" v-model.number="pomodoroDuration" min="1" />
      </label>
      <label>
        Break Duration (minutes):
        <input type="number" v-model.number="breakDuration" min="1" />
      </label>
      <button @click="applySettings" class="btn apply">Apply</button>
    </div>
  </div>
</template>

<style scoped>
.pomodoro-card {
  width: 100%;
  max-width: 600px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 300px;
  padding: 20px;
  margin: 20px 10px;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  background-color: #fff;
  color: #333;
  transition: background-color 0.3s, color 0.3s;
}

.dark-mode .pomodoro-card {
  background-color: #1e1e24;
  color: #fff;
  border: 2px solid #fdf7fa;
}

.title {
  font-size: 1.5rem;
  font-weight: bold;
  margin-bottom: 10px;
}

.status {
  font-size: 1rem;
  font-weight: bold;
  margin-bottom: 20px;
}

.timer {
  font-size: 3rem;
  margin-bottom: 20px;
}

.buttons {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.btn {
  padding: 10px 20px;
  font-size: 1rem;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  background-color: #28a745;
  color: #fff;
  transition: background-color 0.3s;
}

.btn:hover {
  background-color: #218838;
  transform: translateY(-2px);
}

s .dark-mode .btn {
  background-color: #0056b3;
}

.btn.stop {
  background-color: #ef6461;
}

.btn.stop:hover {
  background-color: #c44846;
  transform: translateY(-2px);
}

.dark-mode .btn.stop {
  background-color: #ef6461;
}

.dark-mode .btn.stop:hover {
  background-color: #c44846;
}

.btn.apply {
  background-color: #007bff;
}

.btn.apply:hover {
  background-color: #0056b3;
  transform: translateY(-2px);
}

.dark-mode .btn.apply {
  background-color: #007bff;
}

.dark-mode .btn.apply:hover {
  background-color: #0056b3;
}

.settings {
  display: flex;
  flex-direction: column;
  gap: 10px;
  width: 100%;
}

.settings label {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 1rem;
  width: 100%;
}

.settings input {
  width: 60px;
  padding: 5px;
  border: 1px solid #ddd;
  border-radius: 5px;
  font-size: 1rem;
  transition: border-color 0.3s;
}

.settings input:focus {
  border-color: #fdf7fa;
}

.dark-mode input {
  background-color: #1e1e24;
  border-color: #666;
  color: #ddd;
}
</style>
