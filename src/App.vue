<template>
  <div id="app">
    <FuzzySearch/>
  </div>
</template>

<script>
import FuzzySearch from './components/FuzzySearch.vue'


export default {
  name: 'app',
  components: {
    FuzzySearch
  }
}
</script>

<style>
@import "https://cdn.materialdesignicons.com/2.5.94/css/materialdesignicons.min.css";
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
