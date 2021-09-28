<template>
  <div id="app">
    <h1 class="bg-danger text-white mb-0 py-2">Sławomir Białynicki-Birulo</h1>
    <Slider msg="Welcome to Your Vue.js App"/>
    <h2 class="bg-danger text-white py-2">Zadanie Rekrutacyjne</h2>
  </div>
</template>

<script>
import Slider from './components/Slider.vue'

export default {
  name: 'App',
  components: {
    Slider
  }
}
</script>

<style lang="scss">
#app {
  font-family: monospace;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
