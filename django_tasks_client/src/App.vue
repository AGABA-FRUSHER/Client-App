<template>
  <div id="app">
    <TasksComponent />
  </div>
</template>

<script>
import TasksComponent from './components/TasksComponent.vue'


export default {
  name: 'App',
  components: {
    TasksComponent,
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
