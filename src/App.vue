<template>
  <div id="app">
    <directories></directories>
  </div>
</template>

<script>
import directories from './components/Directories.vue'

export default {
  name: 'App',
  components: {
    'directories' : directories
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Work+Sans:wght@600&display=swap');

* {
  box-sizing: border-box;
  margin: 0;
}
body {
  margin: 0;
  padding: 0;
  background-color: #1F4068;
}
#app {
  font-family: 'Work Sans', sans-serif;
  color: #EBECF1;
}
</style>
