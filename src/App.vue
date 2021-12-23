<template>
  <div id="app">
    <div class="wrapper">
      <div class="inputWrapper">
        <span class="historyIcon">
          <svg focusable="false" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M13 3a9 9 0 0 0-9 9H1l3.89 3.89.07.14L9 12H6c0-3.87 3.13-7 7-7s7 3.13 7 7-3.13 7-7 7c-1.93 0-3.68-.79-4.94-2.06l-1.42 1.42A8.954 8.954 0 0 0 13 21a9 9 0 0 0 0-18zm-1 5v5l4.28 2.54.72-1.21-3.5-2.08V8H12z"></path></svg>
        </span>
        <input type="text" :value="input">
        <span class="calcString" v-show="calcString.length > 0 && this.initialised">{{ calcString }}</span>
        <span class="calcString" v-show="calcResult.length > 0 && !this.initialised">Ans = {{ calcResult }}</span>
      </div>
      <div class="buttonsWrapper">
        <button class="superSpecialButton">
          <span>Rad</span>
          <span class="spacer"></span>
          <span>Deg</span>
        </button>
        <button>x!</button>
        <button>(</button>
        <button>)</button>
        <button class="largeFont">%</button>
        <button @click="clearInput()">AC</button>
        <button>Inv</button>
        <button>sin</button>
        <button>ln</button>
        <button class="whiteButton mediumFont" @click="inputCalc('7')">7</button>
        <button class="whiteButton mediumFont" @click="inputCalc('8')">8</button>
        <button class="whiteButton mediumFont" @click="inputCalc('9')">9</button>
        <button class="largeFont">÷</button>
        <button>π</button>
        <button>cos</button>
        <button>log</button>
        <button class="whiteButton mediumFont" @click="inputCalc('4')">4</button>
        <button class="whiteButton mediumFont" @click="inputCalc('5')">5</button>
        <button class="whiteButton mediumFont" @click="inputCalc('6')">6</button>
        <button class="largeFont" @click="inputCalc(' × ')">×</button>
        <button>e</button>
        <button>tan</button>
        <button>√ </button>
        <button class="whiteButton mediumFont" @click="inputCalc('1')">1</button>
        <button class="whiteButton mediumFont" @click="inputCalc('2')">2</button>
        <button class="whiteButton mediumFont" @click="inputCalc('3')">3</button>
        <button class="largeFont" @click="inputCalc(' - ')">−</button>
        <button>Ans</button>
        <button>EXP</button>
        <button>
          <span>x<sup>y</sup></span>
        </button>
        <button class="whiteButton mediumFont" @click="inputCalc('0')">0</button>
        <button class="whiteButton decimal largeFont" @click="inputCalc('.')">.</button>
        <button class="blueButton largeFont" @click="submit()">=</button>
        <button class="largeFont" @click="inputCalc(' + ')">+</button>
      </div>
    </div>
  </div>
</template>

<script>
import * as math from 'mathjs'

export default {
  name: 'App',
  data () {
    return {
      input: '0',
      total: '',
      calcString: '',
      calcResult: '',
      initialised: true
    }
  },
  methods: {
    inputCalc (num) {
      this.initialised = false
      this.input =
        this.input === '0' || (this.calcString && !isNaN(num)) ? num : `${this.input}${num}`
      this.calcString = ''
    },
    clearInput () {
      this.input = '0'
      this.calcString = this.calcResult ? `Ans = ${this.calcResult}` : null
    },
    submit () {
      let evalString = this.input.replace(/\s/g, '').replace(/×/g, '*').replace(/÷/g, '/')
      this.calcString = `${this.input} =`
      let result = math.evaluate(evalString)
      this.calcResult = math.format(result, {precision: 14}).toString()
      this.input = this.calcResult
      this.initialised = true
    }
  }
}
</script>

<style>
html, * {
  font-family: 'Arial', sans-serif;
  font-size: 13px;
}

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  align-items: center;
  justify-content: center;
}

input {
  width: 96%;
  height: 47px;
  border: 1px solid #ebebeb;
  padding: 23px 14px 0 10px;
  border-radius: 8px;
  text-align: right;
  font-size: 30px;
}

.inputWrapper {
  position: relative;
}

.wrapper {
  width: 652px;
}

.wrapper:hover input {
  border-top: 1px solid;
}

.buttonsWrapper {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  grid-gap: 8px;
  margin-top: 8px;
}

.calcString {
  position: absolute;
  width: 100%;
  left: -16px;
  right: 0;
  z-index: 99;
  top: 12px;
  text-align: right;
  color: #70757a;
}

.superSpecialButton {
  grid-column: 1 / 3;
  width: 100%;
}

.whiteButton {
  background: #f1f3f4;
  border: 1px solid #f1f3f4;
}

.whiteButton:active {
  border-color: #dadce0;
}

.decimal {
  font-weight: bold;
}

.blueButton {
  background: #4285f4;
  color: #fff;
  border: 1px solid #4285f4;
  font-weight: bold;
}

.blueButton:active {
  background: #4285f4;
  border-color: #1558d6;
  box-shadow: 0 1px 2px 0 rgb(66 133 244 / 45%), 0 3px 6px 2px rgb(66 133 244 / 30%);
}

.largeFont {
  font-size: 18px;
}

.mediumFont {
  font-size: 14px;
}

button {
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 400;
  height: 36px;
  line-height: 26px;
  background: #dadce0;
  color: #202124;
  border: 1px solid #dadce0;
  border-radius: 4px;
}

button:active {
  box-shadow: 0 1px 6px rgb(32 33 36 / 28%);
  border-color: #70757a;
}

button:hover {
  cursor: pointer;
}

sup {
  font-size: 8px;
    line-height: 8px;
}

.historyIcon {
  display: block;
  position: absolute;
  width: 22px;
  padding: 10px 0 0 10px;
  fill: #70757a;
}

.superSpecialButton {
  display: flex;
  justify-content: space-around;
}

.spacer {
  background: #70757a;
  width: 1px;
  height: 16px
}

</style>
