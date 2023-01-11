<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <GameBlock/>
</template>

<script>
import GameBlock from './components/GameBlock.vue'

export default {
  name: 'App',
  components: {
    GameBlock
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
