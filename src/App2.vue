<script setup>
import { ref, onMounted } from 'vue';

const name = ref('John');
const status = ref('pending');
const tasks = ref(['Task 1', 'Task 2', 'Task 3']);
const newTask = ref('');

const toggleStatus = () => {
  if (status.value === 'active') {
    status.value = 'inactive';
  } else if (status.value === 'inactive') {
    status.value = 'pending';
  } else {
    status.value = 'active';
  }
};

const addTask = () => {
  if (newTask.value.trim() !== '') {
    tasks.value.push(newTask.value.trim());
    newTask.value = '';
  }
};

const deleteTask = (index) => {
  tasks.value.splice(index, 1);
};

onMounted(async ()  => {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/todos?_limit=15');
    const data = await response.json();
    tasks.value = data.map(item => item.title);
  } catch (error) {
    console.error('Error fetching tasks:', error);
  }
});

</script>

<template>
  <h1>{{ name }} Doe</h1>
  <p v-if="status === 'active'">User is active</p>
  <p v-else-if="status === 'pending'">User is pending</p>
  <p v-else>User is inactive</p>
  
  <form @submit.prevent="addTask">
    <input v-model="newTask" placeholder="New Task" />
    <button type="submit">Add Task</button>
  </form>

  <h3>Tasks:</h3>
  <ul>
    <li v-for="(task, index) in tasks" :key="task">
      <span>{{ task }}</span>
      <button @click="deleteTask(index)">X</button>
    </li>
  </ul>
  <br />
  <button @click="toggleStatus">Change status</button>
</template>

<style scoped>
  
</style>