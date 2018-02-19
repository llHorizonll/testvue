<template>
  <div id="app">
    <Board/>
    <!-- <Chioce/> -->
  </div>
</template>

<script>
import Board from './components/Board'
// import Chioce from './components/Chioce'

export default {
  name: 'App',
  components: {
    Board,
    // Chioce
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
}
</style>
