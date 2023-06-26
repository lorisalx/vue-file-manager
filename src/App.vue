<template>
  <div id="app">
    <h1>File Manager</h1>
    <FileManager/>
  </div>
</template>

<script>
import FileManager from './components/FileManager.vue'

export default {
  name: 'App',
  components: {
    FileManager
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin: 5%;
}
</style>
