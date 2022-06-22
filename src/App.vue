<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import FilterCSV from './components/FilterCSV.vue';
</script>

<template>
  <img alt="File Icon" src="./assets/file-icon.svg" width="150" />
  <FilterCSV />
  <footer>Con ❤️ para mi Sofía</footer>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

footer {
  color: #333;
  position: absolute;
  width: 100%;
  bottom: 0;
  font-size: 12px;
  text-align: center;
}
</style>
