<script setup>
//composition api
import { ref, onMounted } from 'vue'
    const name = ref('ibrahim adham');
    const status = ref('active');
    const tasks = ref(['task one', 'task two', 'task three']);
    const newTask = ref('swsws');

    const toggleStatus = () => {
      if (status.value === 'active') {
        status.value = 'pending'; 
      } else if (status.value === 'pending') {
        status.value = 'inactive';
      } else {
        status.value = 'active';
      }
    }

    const  addTask = () => {
      if (newTask.value.trim() !== '') {
        tasks.value.push(newTask.value);
        newTask.value = '';
      }
      
    };

    const deleteTask = (index) => {
      tasks.value.splice(index, 1);
    }

onMounted (async () => {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/todos');
    const data = await response.json();
    tasks.value = data.map((task) => task.title);
  } catch(error) {
    console.log('error fetch');
  }
}) 

</script>


<template>
  <h1>{{ name }}</h1>
  <p v-if="status === 'active'">User is Active</p>
  <p v-else-if="status === 'pending'">User is Pending</p>
  <p v-else>User Inactive</p>

  <form @submit.prevent="addTask">
    <label for="newTask">Add Task</label>
    <input type="text" id="newTask" name="newTask" v-model="newTask">
    <button type="submit">Submit</button>
  </form>


  <br />
  <h3>Tasks:</h3>
  <ul>
    <li v-for="(task, index) in tasks" :key="task">
      <span>
        {{ task }}
      </span>
      <button @click="deleteTask(index)">Delete Me</button>
    </li>
  </ul>

<br>
  <button @click="toggleStatus">Change Status</button>
</template>

<style scoped>
  h1 {
    color: red;
  }
</style>