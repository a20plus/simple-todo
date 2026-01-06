<script setup>
import "/assets/css/main.css";

const tasks = ref([]);
const newTask = ref("");

const addTask = () => {
  if (newTask.value.trim() !== "") {
    tasks.value.push(newTask.value);
    newTask.value = "";
  }
};

const doneTask = (index) => {
  tasks.value.splice(index, 1);
};

onMounted(async()=>{
  try {
    const respoinse = await fetch("https://jsonplaceholder.typicode.com/todos");
    const data = await respoinse.json();
    tasks.value = data.map((task) => task.title)
  } catch (error) {
    console.error(error)
  }
})
</script>

<template>
  <h1>To do list</h1>
  <form @submit.prevent="addTask">
    <label for="newTask">Add Task</label>
    <input type="text" id="newTask" name="newTask" v-model="newTask" />
    <button type="submit">submit</button>
  </form>
  <h3>tasks:</h3>
  <ul>
    <li v-for="(task, index) in tasks">
      <span>{{ task }}</span>
      <button @click="doneTask(index)">Done</button>
    </li>
  </ul>
</template>

<style scoped>
h1 {
  font-size: 80px;
  font-weight: bold;
}

input {
  background-color: white;
  padding: 10px;
  margin: 0 10px;
  border: 1px solid blue;
  border-radius: 5px;
}

button {
  padding: 10px;
  background-color: white;
  cursor: pointer;
  border-radius: 5px;
}
</style>
