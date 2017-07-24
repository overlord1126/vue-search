<template>
  <div id="app">
    <search-box></search-box>
  </div>
</template>

<script>
import searchBox from "./components/getData.vue" ;
//import searchBox from "./components/search.vue" ;
//import searchBox from "./components/index.vue" ;
export default {
  name: 'app',
  components:{
  	"search-box":searchBox
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
