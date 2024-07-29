<script setup>
import { ref, onMounted } from 'vue';

    const name = ref('John Doe');
    const status = ref('active');
    const tasks = ref(['Task One', 'Task Two', 'Task Three']);
    const newTask = ref('');

    const toogleStatus = () => {
      if(status.value === 'active') {
        status.value = 'pending';
      } else if(status.value === 'pending') {
        status.value = 'inactive';
      } else {
        status.value = 'active'
      }
    };

    const addtask = () => {
      if (newTask.value.trim() != '') {
        tasks.value.push(newTask.value);
        newTask.value = '';
      }
    };

    const deleteTask = (index) => {
      tasks.value.splice(index, 1);
    }



  onMounted(async () =>{
    try {
      const response = await fetch('https://jsonplaceholder.typicode.com/todos');
      const data = await response.json();
      tasks.value = data.map((tasks) => tasks.title)
    } catch (error) {
      console.log('Error fetching tasks')
    }
  });
</script>



<template>
  <h1>{{name}}</h1>
  <p v-if="status === 'active'"> User is Active. </p>
  <p v-else-if="status ==='pending'"> User is Pending</p>
  <p v-else> User is Inactive.</p>

  <form @submit.prevent="addtask" action="">
    <label for="newtask"> Add Task</label>
    <input type="text" id="newTask" name="newTask" v-model="newTask" />
    <button type="submit"> Submit </button>
  </form>



  <h3> Tasks: </h3>
  <ul>
    <li v-for="(task, index) in tasks" :key="task">
      <span>
        {{ task }}
      </span>
      <button @click="deleteTask()"> X </button>
    </li>
  </ul>

  <br>

  <button @click="toogleStatus"> Change Status </button>




</template>

