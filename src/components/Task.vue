<template>
  <li class="task-item">
    <p class="task-text" :class="{ completed: isCompleted }">{{ task.text }}</p>
    <div class="task-actions">
      <button @click="completeTask" class="btn complete-task-btn">
        <i :class="task.completed ? 'fas fa-undo' : 'fas fa-check'"></i>
      </button>
      <button @click="removeTask" class="btn remove-task-btn">
        <i class="fas fa-trash"></i>
      </button>
    </div>
  </li>
</template>
<script lang="ts" setup>
import { defineProps, defineEmits, ref } from 'vue';

interface TaskProps {
  task: {
    id: number;
    text: string;
    completed: boolean;
  };
}

const props = defineProps<TaskProps>();
const emit = defineEmits<{
  (e: 'remove-task', id: number): void;
  (e: 'complete-task', id: number): void;
}>();

const isCompleted = ref(props.task.completed);

const completeTask = () => {
  isCompleted.value = !isCompleted.value;
  emit('complete-task', { id: props.task.id, completed: isCompleted.value });
};

const removeTask = () => {
  emit('remove-task', props.task.id);
};
</script>

<style scoped>
.task-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 8px;
  background-color: #f9f9f9;
  transition: background-color 0.3s;
  margin-bottom: 10px;
}

.task-text {
  flex: 1;
  overflow-wrap: break-word;
  word-wrap: break-word;
  margin-right: 10px;
  white-space: pre-wrap;
  word-break: break-word;
}

.task-actions {
  display: flex;
  gap: 10px;
}

.completed {
  text-decoration: line-through;
  color: #888;
}

.complete-task-btn {
  background-color: #28a745;
  color: #fff;
  border-radius: 4px;
  border: none;
  cursor: pointer;
  padding: 6px;
}

.complete-task-btn:hover {
  background-color: #218838;
}

.remove-task-btn {
  background-color: #dc3545;
  color: #fff;
  border-radius: 4px;
  border: none;
  cursor: pointer;
  padding: 6px;
}

.remove-task-btn:hover {
  background-color: #c82333;
}
</style>
