<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <CsvReader/>
  </div>
</template>

<script>
import CsvReader from './components/CsvReader.vue';

export default {
  name: 'App',
  components: {
    CsvReader
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
