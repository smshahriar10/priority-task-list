<!-- Github repo: https://github.com/smshahriar10/priority-task-list.git -->

<!-- Template code: -->

<script setup>
import { ref, computed, onMounted, watch } from 'vue';

const taskName = ref('');
const taskPriority = ref(''); // We'll set this to an empty string initially
const tasks = ref([]);

// Load tasks from localStorage on mount
onMounted(() => {
  const storedTasks = localStorage.getItem('priorityTasks');
  if (storedTasks) {
    tasks.value = JSON.parse(storedTasks);
  }
});

// Watch for changes in tasks and update localStorage
watch(tasks, (newTasks) => {
  localStorage.setItem('priorityTasks', JSON.stringify(newTasks));
}, { deep: true });

// Computed properties for each priority level
const highPriorityTasks = computed(() => {
  return tasks.value
    .filter(task => task.priority === 'High')
    .sort((a, b) => a.name.localeCompare(b.name));
});

const mediumPriorityTasks = computed(() => {
  return tasks.value
    .filter(task => task.priority === 'Medium')
    .sort((a, b) => a.name.localeCompare(b.name));
});

const lowPriorityTasks = computed(() => {
  return tasks.value
    .filter(task => task.priority === 'Low')
    .sort((a, b) => a.name.localeCompare(b.name));
});

// Add a new task
function addTask() {
  if (taskName.value.trim() === '' || taskPriority.value === '') return;
  tasks.value.push({
    name: taskName.value.trim(),
    priority: taskPriority.value
  });
  taskName.value = '';
  taskPriority.value = '';
}
</script>

<template>
  <div class="app-container">
    <h1>Priority Task List</h1>
    <form @submit.prevent="addTask" class="add-task-form">
      <input
        v-model="taskName"
        type="text"
        placeholder="Task Name"
        class="task-input"
      />
      <select v-model="taskPriority" class="task-select">
        <option value="">Select Priority</option>
        <option>High</option>
        <option>Medium</option>
        <option>Low</option>
      </select>
      <button type="submit" class="add-btn">Add Task</button>
    </form>

    <div class="task-section">
      <h2>High Priority</h2>
      <ul v-if="highPriorityTasks.length > 0">
        <li v-for="(task, index) in highPriorityTasks" :key="'high-'+index">
          {{ task.name }}
        </li>
      </ul>
      <p v-else>No tasks available in this category</p>
    </div>

    <div class="task-section">
      <h2>Medium Priority</h2>
      <ul v-if="mediumPriorityTasks.length > 0">
        <li v-for="(task, index) in mediumPriorityTasks" :key="'med-'+index">
          {{ task.name }}
        </li>
      </ul>
      <p v-else>No tasks available in this category</p>
    </div>

    <div class="task-section">
      <h2>Low Priority</h2>
      <ul v-if="lowPriorityTasks.length > 0">
        <li v-for="(task, index) in lowPriorityTasks" :key="'low-'+index">
          {{ task.name }}
        </li>
      </ul>
      <p v-else>No tasks available in this category</p>
    </div>
  </div>
</template>

<style scoped>
body {
  margin: 0;
  font-family: Arial, sans-serif;
  background: #fff;
  color: #333;
}

.app-container {
  max-width: 500px;
  margin: 0 auto;
  padding: 40px 20px;
  text-align: center;
}

h1 {
  font-size: 36px;
  margin-bottom: 30px;
  font-weight: bold;
  color: #333;
}

.add-task-form {
  display: flex;
  flex-direction: column;
  margin-bottom: 40px;
}

.task-input,
.task-select {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  margin-bottom: 10px;
  font-size: 16px;
}

.add-btn {
  background-color: #6c38d9; /* A nice purple shade */
  color: #fff;
  padding: 10px;
  border-radius: 4px;
  border: none;
  font-size: 16px;
  font-weight: bold;
  cursor: pointer;
  text-transform: uppercase;
}

.add-btn:hover {
  background-color: #5930af;
}

.task-section {
  margin-bottom: 40px;
}

.task-section h2 {
  font-size: 24px;
  margin-bottom: 10px;
  font-weight: bold;
}

.task-section p {
  color: #666;
  margin: 0;
  font-style: italic;
  font-size: 14px;
}

.task-section ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.task-section li {
  background: #f5f5f5;
  padding: 10px;
  border-radius: 4px;
  margin-bottom: 5px;
  text-align: left;
  border: 1px solid #ccc;
  font-size: 16px;
  color: #333;
}
</style>