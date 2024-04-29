<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <pokedex></pokedex>
</template>

<script>
import pokemon from './components/pokedexVista.vue'

export default {
  name: 'App',
  components: {
    pokedex
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
