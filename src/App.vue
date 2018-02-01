<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
import tachions from 'tachyons/css/tachyons.min.css'
export default {
  name: 'App',
  tachions
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
