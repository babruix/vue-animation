<template>
  <div class="hello">

    <div class="boo" v-bind:style="booStyle"></div>
    <controls v-on:updatedControls="updateControls"></controls>

  </div>
</template>

<script>
  import Controls from './Controls'

  export default {
    name: 'animation',
    components: {
      Controls
    },
    data () {
      return {
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
      updateControls: function(newValues) {
        this.dotsCount = newValues.dotsCount;
        this.colorsCount = newValues.colorsCount;
        this.colorsRepeat = newValues.colorsRepeat;
        this.cutOff = newValues.cutOff;
        this.diam = newValues.diam;
        this.scaleFactor = newValues.scaleFactor;
        this.cutOffFactor = newValues.cutOffFactor;
        this.multiply = newValues.multiply;
        this.cutOffShift = newValues.cutOffShift;
      },
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
        // @todo: Is there better way to do it?
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
