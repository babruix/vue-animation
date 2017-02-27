<template>
  <div>
    <transition name="bounce">
      <div id="controls" v-if="showControls">

        <div>
          <span>Number of dots: {{dotsCount}}</span>
          <range-slider
            class="slider"
            min="1"
            max="200"
            step="1"
            v-model="dotsCount"
            @change="onSliderChange">
          </range-slider>
        </div>


        <div>
          <span>Number of colors: {{colorsCount}}</span>
          <range-slider
            class="slider"
            min="1"
            max="20"
            step="1"
            v-model="colorsCount"
            @change="onSliderChange">
          </range-slider>
        </div>


        <div>
          <span>Repeating of colors: {{colorsRepeat}}</span>
          <range-slider
            class="slider"
            min="1"
            max="20"
            step="1"
            v-model="colorsRepeat"
            @change="onSliderChange">
          </range-slider>
        </div>


        <div>
          <span>Cutoff: {{cutOff | fixedTwo}}</span>
          <range-slider
            class="slider"
            min="0.05"
            max="1"
            step=".05"
            v-model="cutOff"
            @change="onSliderChange">
          </range-slider>
        </div>


        <div>
          <span>Cutoff Factor: {{cutOffFactor | fixedTwo}} </span>
          <range-slider
            class="slider"
            min="0"
            max="8"
            step=".01"
            v-model="cutOffFactor"
            @change="onSliderChange">
          </range-slider>
        </div>


        <div>
          <span>Diameter: {{diam | fixedTwo}}</span>
          <range-slider
            class="slider"
            min="0.02"
            max="1"
            step=".01"
            v-model="diam"
            @change="onSliderChange">
          </range-slider>
        </div>


        <div>
          <span>Scale: {{scaleFactor}}</span>
          <range-slider
            class="slider"
            min="1"
            max="40"
            step="1"
            v-model="scaleFactor"
            @change="onSliderChange">
          </range-slider>
        </div>


        <div>
          <span>Multiply: {{multiply}}</span>
          <range-slider
            class="slider"
            min="2"
            max="8"
            step="1"
            v-model="multiply"
            @change="onSliderChange">
          </range-slider>
        </div>


        <div>
          <span>Cutoff Shift: {{cutOffShift}}</span>
          <range-slider
            class="slider"
            min="0"
            max="8"
            step="1"
            v-model="cutOffShift"
            @change="onSliderChange">
          </range-slider>
        </div>


      </div>
    </transition>

    <input id="showControls" type="checkbox" v-model="showControls">
    <label for="showControls" id="showControlsLabel">Controls</label>

  </div>
</template>

<script>
  import RangeSlider from 'vue-range-slider'
  import 'vue-range-slider/dist/vue-range-slider.css'

  export default {
    name: 'controls',
    components: {
      RangeSlider
    },
    data () {
      return {
        showControls: true,
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
      onSliderChange () {
        this.$emit('updatedControls', {
          'dotsCount': this.dotsCount,
          'colorsCount': this.colorsCount,
          'colorsRepeat': this.colorsRepeat,
          'cutOff': this.cutOff,
          'diam': this.diam,
          'scaleFactor': this.scaleFactor,
          'cutOffFactor': this.cutOffFactor,
          'multiply': this.multiply,
          'cutOffShift': this.cutOffShift
        });
      },
    }
  }

</script>

<style scoped>
  #controls {
    position: fixed;
    top: 20px;
    text-align: left;
    padding-left: 10px;
    border: 1px solid #21fb92;
    background-color: white;
    box-sizing: border-box;
    box-shadow: 0 2px 5px 0 rgba(0, 0, 0, 0.16), 0 2px 10px 0 rgba(0, 0, 0, 0.12);
  }

  #showControls, #showControlsLabel {
    position: fixed;
    top: 0;
    left: 0;
  }
  #showControlsLabel {
    left: 17px;
    cursor: pointer;
  }

  .bounce-enter-active {
    animation: bounce-in .5s;
  }
  .bounce-leave-active {
    animation: bounce-out .5s;
  }
  @keyframes bounce-in {
    0% {
      transform: scale(0);
    }
    50% {
      transform: scale(1.5);
    }
    100% {
      transform: scale(1);
    }
  }
  @keyframes bounce-out {
    0% {
      transform: scale(1);
    }
    50% {
      transform: scale(1.5);
    }
    100% {
      transform: scale(0);
    }
  }
</style>
