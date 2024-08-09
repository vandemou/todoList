<template>
  <div>
    <h1>My To-Do List</h1>
    <ToDoList />
  </div>
</template>

<script>
import ToDoList from './components/ToDoList.vue';

export default {
  components: {
    ToDoList
  }
};
</script>

<style scoped>
body {
  font-family: 'Arial', sans-serif;
  background-color: #f4f7f6;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

#app {
  text-align: center;
}

h1 {
  color: #2c3e50;
  font-weight: bold;
  margin-bottom: 20px;
}

.todo-container {
  background: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  width: 100%;
  max-width: 400px;
}
</style>