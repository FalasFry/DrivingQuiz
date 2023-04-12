<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <HelloWorld msg="Welcome To This Quiz App"/>
</template>

<script>
import HelloWorld from './components/quiz.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #7592af;
  margin-top: 60px;
}

body {
  background-color: #252525;
}
</style>
