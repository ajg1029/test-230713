<template>
  <div id="app">
    <Test001 />
  </div>
</template>

<script>
import Test001 from './components/Test001.vue'

export default {
  name: 'App',
  components: {
    Test001
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
