<template xmlns:v-bind="http://www.w3.org/1999/xhtml">
  <div class="hello">

    <h1 id="testAnim">{{ msg }}</h1>


    <div>
      <span>Number of dots: {{dotsCount}}</span>
      <range-slider
        class="slider"
        min="1"
        max="200"
        step="1"
        v-model="dotsCount">
      </range-slider>
    </div>


    <div>
      <span>Number of colors: {{colorsCount}}</span>
      <range-slider
        class="slider"
        min="1"
        max="20"
        step="1"
        v-model="colorsCount">
      </range-slider>
    </div>


    <div>
      <span>Repeating of colors: {{colorsRepeat}}</span>
      <range-slider
        class="slider"
        min="1"
        max="20"
        step="1"
        v-model="colorsRepeat">
      </range-slider>
    </div>


    <div>
      <span>Cutoff: {{cutOff}}</span>
      <range-slider
        class="slider"
        min="0"
        max="1"
        step=".05"
        v-model="cutOff">
      </range-slider>
    </div>


    <div>
      <span>Cutoff Factor: {{cutOffFactor}}</span>
      <range-slider
        class="slider"
        min="0"
        max="8"
        step=".01"
        v-model="cutOffFactor">
      </range-slider>
    </div>


    <div>
      <span>Diameter: {{diam}}</span>
      <range-slider
        class="slider"
        min="0.02"
        max="1"
        step=".01"
        v-model="diam">
      </range-slider>
    </div>


    <div>
      <span>Scale: {{scaleFactor}}</span>
      <range-slider
        class="slider"
        min="1"
        max="40"
        step="1"
        v-model="scaleFactor">
      </range-slider>
    </div>


    <div>
      <span>Multiply: {{multiply}}</span>
      <range-slider
        class="slider"
        min="2"
        max="8"
        step="1"
        v-model="multiply">
      </range-slider>
    </div>


    <div>
      <span>Cutoff Shift: {{cutOffShift}}</span>
      <range-slider
        class="slider"
        min="0"
        max="8"
        step="1"
        v-model="cutOffShift">
      </range-slider>
    </div>


    <button id="clickBtn" v-on:click="onClicked"> click!</button>

    <div class="boo" v-bind:style="booStyle"></div>
    <div>
      <ul> Credits:
        <li v-for="user in credits">
          <div>
            {{ user.firstName }} {{ user.lastName }}
          </div>
        </li>
      </ul>

    </div>
  </div>
</template>

<script>

  import {css, tween, easing, colorTween} from 'popmotion';
  import RangeSlider from 'vue-range-slider'
  import 'vue-range-slider/dist/vue-range-slider.css'

  export default {
    name: 'animation',
    components: {
      RangeSlider
    },
    data () {
      return {
        msg: 'Animation with Vue.js',
        credits: [
          {firstName: 'Alexey', lastName: 'Romanov'},
        ],
        dotsCount: 26,
        colorsCount: 7,
        colorsRepeat: 3,
        cutOff: .25,
        diam: 0.125,
        scaleFactor: 17.5,
        cutOffFactor: 4,
        multiply: 2,
        cutOffShift: 1,
      }
    },
    methods: {
      onClicked: () => {
        const buttonRenderer = css(document.getElementById('clickBtn'));

        tween({
          to: Math.floor(Math.random() * 100),
          ease: easing.backInOut,
          onUpdate: (v) => buttonRenderer.set('x', v)
        }).start();

        const testAnim = css(document.getElementById('testAnim'));

        colorTween({
          from: 'rgb(0,200,255)',
          to: 'rgb(100,250,0)',
          yoyo: Infinity,
          ease: easing.linear,
          duration: 3000,
          onUpdate: (v) => testAnim.set('boxShadow', 'inset 0 0 15px ' + v)
        }).start();
      }
    },

    computed: {

      booStyle() {

        let n = this.colorsCount
          , m = this.colorsRepeat
          , p = this.dotsCount
          , d = this.diam
          , cutoff = this.cutOff
          , boxShadowInitial = ''
          , boxShadowTo = ''
          , multiply = this.multiply;

        for (let i = 0, totalColors = n * m; i <= totalColors; i++) {
          let betaAngle = i * multiply * Math.PI / totalColors
            , colorHue = i * 360 / n
            , x = 0
            , y = 0
            , xFrom = 0
            , yFrom = 0
            , xTo = 0
            , yto = 0;

          for (let j = 1; j <= p; j++) {
            let gammaAngle = j * multiply * Math.PI / p
              , s = this.scaleFactor
              * (cutoff * p - j)
              * (j - (this.cutOffShift + cutoff / this.cutOffFactor) * p)
              / Math.pow((0.5 + cutoff) * p, multiply);

            if (j < cutoff * p) {
              s = 0;
            }

            x = x + 1.65 * (s + 1) * d;
            y = x * Math.sin(gammaAngle) / 5;

            xFrom = x * Math.cos(betaAngle) - y * Math.sin(betaAngle);
            yFrom = x * Math.sin(betaAngle) + y * Math.cos(betaAngle);

            if (boxShadowInitial.length > 0) {
              boxShadowInitial += ', ';
            }
            boxShadowInitial += `${xFrom}em ${yFrom}em 0 ${s * d}em hsl(${colorHue}, 100%, 50%)`;

            xTo = x * Math.cos(betaAngle + gammaAngle);
            yto = x * Math.sin(betaAngle + gammaAngle);
            if (boxShadowTo.length > 0) {
              boxShadowTo += ', ';
            }
            boxShadowTo += `${xTo}em ${yto}em 0 ${d / multiply}em hsl(${colorHue}, 100%, 50%)`;
          }
        }

        // Inject animation
        // @todo: is there better way to do it?
        let style = document.createElement('style');
        style.type = 'text/css';
        style.innerHTML = `
        @keyframes ani {
          to {
            box-shadow: ${boxShadowTo};
          }
        }`;
        document.getElementsByTagName('head')[0].appendChild(style);


        return {
          'margin': -d / 2 + 'em',
          'width': d + 'em',
          'height': d + 'em',
          'box-shadow': boxShadowInitial,
          'position': 'absolute',
          'top': '50%',
          'left': '50%',
          'border-radius': '50%',
          'background-color': 'black',
          'animation': 'ani 2s ease-in-out infinite alternate'
        }
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  h1, h2 {
    font-weight: normal;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    display: inline-block;
    margin: 0 10px;
  }

  a {
    color: #42b983;
  }

  #testAnim {
    padding: 1px;
  }

</style>
