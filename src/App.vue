<template>
  <div id="app">
    <router-view/>
    <rili/>
  </div>
</template>

<script>
import rili from '@/components/rili'
export default {
  name: 'App',
  components:{
    rili
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
.swi{
 margin: 900px 300px
}
</style>
