<template>
  <div :class="['task-manager', { 'dark-mode': isDarkMode }]">
    <h2 class="task-title">Task Manager</h2>
    <div class="task-input">
      <input
        v-model="newTask"
        placeholder="Add a new task"
        @keyup.enter="addTask"
      />
      <button @click="addTask" class="btn add-task-btn">
        <i class="fa fa-plus"></i>
      </button>
    </div>
    <ul class="task-list">
      <Task
        v-for="task in tasks"
        :key="task.id"
        :task="task"
        @remove-task="removeTask"
        @complete-task="completeTask"
      />
    </ul>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import Task from './Task.vue';

interface Task {
  id: number;
  text: string;
  completed: boolean;
}

const newTask = ref('');
const tasks = ref<Task[]>([]);
let nextId = 1;
const isDarkMode = ref(false);

const addTask = () => {
  if (newTask.value.trim() === '') return;
  tasks.value.push({
    id: nextId++,
    text: newTask.value.trim(),
    completed: false,
  });
  newTask.value = '';
};

const removeTask = (id: number) => {
  tasks.value = tasks.value.filter((task) => task.id !== id);
};

const completeTask = (task: { id: number; completed: boolean }) => {
  const taskToUpdate = tasks.value.find((t) => t.id === task.id);
  if (taskToUpdate) {
    taskToUpdate.completed = task.completed;
  }
};
const toggleDarkMode = () => {
  isDarkMode.value = !isDarkMode.value;
};
</script>

<style scoped>
.task-manager {
  width: 100%;
  max-width: 600px;
  margin: 20px;
  padding: 20px;
  border: 2px solid #ddd;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  background-color: #fff;
  color: #333;
  transition: background-color 0.3s, color 0.3s;
}

.task-title {
  font-size: 1.5rem;
  margin-bottom: 15px;
}

.task-input {
  display: flex;
  gap: 10px;
  margin-bottom: 15px;
}

.task-input input {
  flex: 1;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.task-input button {
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  background-color: #007bff;
  color: #fff;
  transition: background-color 0.3s;
}

.task-input button:hover {
  background-color: #0056b3;
}

.task-list {
  list-style: none;
  padding: 0;
  margin: 0;
  max-height: 600px;
  overflow-y: auto;
}

.task-list li {
  margin-bottom: 10px;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 8px;
  background-color: #f9f9f9;
  transition: background-color 0.3s;
}

.task-list li.completed {
  background-color: #e0e0e0;
}

.completed {
  text-decoration: line-through;
  color: #888;
}

/* Dark mode styles */

.dark-mode .task-manager {
  background-color: #1e1e24;
  color: white;
  border: 2px solid #fdf7fa;
}

.dark-mode .task-input input {
  background-color: #1e1e24;
  border-color: #666;
  color: #ddd;
}

.dark-mode .task-input button {
  background-color: #007bff;
  color: #fff;
}

.dark-mode .task-input button:hover {
  background-color: #0056b3;
}

.dark-mode .task-list li {
  background-color: #444140;
  border-color: #666;
}

.dark-mode .completed {
  color: #999;
}
</style>
