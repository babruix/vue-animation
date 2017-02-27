<template>
  <div id="app">
    <img class="logo" src="./assets/logo.png">
    <Animation></Animation>
  </div>
</template>

<script>
import Animation from './components/Animation'

export default {
  name: 'app',
  components: {
    Animation
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
     .logo {
  animation: colorRotate 3s linear infinite;
         z-index: 2;
}

@keyframes colorRotate {
  100% {
    -webkit-filter: hue-rotate(360deg);
  }
}
</style>
