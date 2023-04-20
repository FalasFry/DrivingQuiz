<template>
  <div>
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
  justify-content: center;
  color: #3d3d3d;
}

body {
  background-color: #ccc;
}

button {
  font-size: 20px;
  text-align: center;
  background-color: #7474745e;
  border: 10px;
  border-radius: 10px;
  margin: 5px;
  color: #252525;
  box-shadow: 5px;
  padding: 15px;
  text-decoration: none;
}

@media (display: standalone) {
  #app {
    -webkit-tap-highlight-color: transparent;
  }
  header {
    height: 100px;      
    padding-bottom: 100px;
  }
}


</style>
