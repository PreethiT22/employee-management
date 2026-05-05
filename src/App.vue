<template>
  <div id="app">
    <EmployeeApp />
  </div>
</template>

<script>
import EmployeeApp from './components/EmployeeApp.vue';

export default {
  name: 'App',
  components: {
    EmployeeApp
  }
}
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  background-color: #f0f4f8;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}
</style>