<template>
  <div id="app">
    <h1 id="testAnim">{{ msg }}</h1>
    <img class="logo" src="./assets/logo.png">

    <Animation></Animation>

    <Credits></Credits>
  </div>
</template>

<script>
  import Animation from './components/Animation'
  import Credits from './components/Credits'
  import {css, tween, easing, colorTween} from 'popmotion'

  export default {
    name: 'app',
    components: {
      Animation,
      Credits
    },
    data () {
      return {
        msg: 'Animation controlled with Vue.js'
      }
    },
    mounted: () => {
      const testAnim = css(document.getElementById('testAnim'));

      colorTween({
        from: 'rgb(0,200,255)',
        to: 'rgb(100,250,0)',
        yoyo: Infinity,
        ease: easing.linear,
        duration: 3000,
        onUpdate: (v) => testAnim.set('boxShadow', 'inset 0 0 15px ' + v)
      }).start();
    },
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

  .logo {
    animation: colorRotate 3s linear infinite;
  }

  @keyframes colorRotate {
    100% {
      -webkit-filter: hue-rotate(360deg);
    }
  }

  h1, h2 {
    font-weight: normal;
    width: 25%;
    margin: 0 auto;
  }

  #testAnim {
    padding: 1px;
    top: 0;
    margin-top: 0;
  }
</style>
