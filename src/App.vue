<template>
  <div>
    <h2>Welcome To the practice driving quiz</h2>
    <QuizApp/>
  </div>
</template>

<script>
import QuizApp from './components/QuizApplet.vue'

export default {
  name: 'App',
  components: {
    QuizApp
  },data() {
    return {
      currentComponent: null
    }
  },
  methods: {
    loadComponent() {
      this.currentComponent = QuizApp
    }
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

button {
  font-size: 20px;
  text-align: center;
  background-color: #0000002b;
  border: 10px;
  border-radius: 10px;
  margin: 5px;
  color: #7592af;
  box-shadow: 5px;
  padding: 15px;
}

body {
  background-color: #252525;
}
</style>
