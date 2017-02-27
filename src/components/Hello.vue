<template>
  <div class="hello">
      
    <h1  id="inputElement">{{ msg }}</h1>
      
      
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
      
      
      
          <button id="clickBtn" v-on:click="onClicked"> click! </button>
   
      <div class="boo" v-bind:style="booStyle"> </div>
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

import { css, tween, easing, colorTween } from 'popmotion';
import RangeSlider from 'vue-range-slider'
import 'vue-range-slider/dist/vue-range-slider.css'
    
export default {
  name: 'hello',
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
              to:  Math.floor(Math.random() * 100),
              ease: easing.backInOut,
              onUpdate: (v) => buttonRenderer.set('x', v)
            }).start();
            
              const inputElement = css(document.getElementById('inputElement'));
           
            colorTween({
                from: 'rgb(0,200,255)',
                to: 'rgb(100,250,0)',
                yoyo: Infinity,
                ease: easing.linear,
                duration: 3000,
              onUpdate: (v) =>  inputElement.set('boxShadow', 'inset 0 0 15px ' + v)
            }).start();
        }
    },
    
    computed:{
        
   booStyle() {
       
      let $n = this.colorsCount
      , $m = this.colorsRepeat
      , $p = this.dotsCount
      , $d = this.diam
      , $cutoff = this.cutOff
      , $sh = ''
      , $sh2 = ''
      , $multiply = this.multiply;
       
    for (let $i = 0; $i <= $n * $m; $i++) {
      let $beta = $i * $multiply * Math.PI / ($n*$m)
      , $x = 0
      , $y = 0
      , $x1 = 0
      , $y1 = 0
      , $x2 = 0
      , $y2=0;
        
      for (let $j = 1; $j<= $p; $j++) {
        let $gamma = $j * $multiply * Math.PI / $p
        , $s = this.scaleFactor 
        * ($cutoff * $p - $j) 
        * ($j - (this.cutOffShift + $cutoff/this.cutOffFactor) * $p) 
        / Math.pow((0.5 + $cutoff) * $p, $multiply);
          
        if ($j < $cutoff * $p) {
          $s = 0;
        }
          
        $x = $x + 1.65 * ($s + 1) * $d;
        $y = $x * Math.sin($gamma) / 5;
        $x1 = $x * Math.cos($beta) - $y * Math.sin($beta); // change of coords
        $y1 = $x * Math.sin($beta) + $y * Math.cos($beta);
        $x2 = $x * Math.cos($beta + $gamma);
        $y2 = $x * Math.sin($beta + $gamma);
          if ($sh.length > 0) {
            $sh += ', ';
          }
          if ($sh2.length > 0) {
            $sh2 += ', ';
          }
        $sh += `${$x1}em ${$y1}em 0 ${$s * $d}em hsl(${$i*360/$n}, 100%, 50%)`;
        $sh2 += `${$x2}em ${$y2}em 0 ${$d / $multiply}em hsl(${$i*360/$n}, 100%, 50%)`;
      }
    }

// Inject animation
var style = document.createElement('style');
style.type = 'text/css';
style.innerHTML = `
@keyframes ani {
 to {
  box-shadow: ${$sh2};
 }
}`;
document.getElementsByTagName('head')[0].appendChild(style);
       
       
    return {
        'margin': -$d/2+'em',
        'width': $d+'em',
       'height': $d+'em',
       'box-shadow': $sh,
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
#inputElement {
    padding: 1px;
}

</style>
