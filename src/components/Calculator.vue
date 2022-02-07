<template>
  <div class="container">
    <div class="grid">
      <p v-if="currentOperation">
        {{ firstNum }} {{ currentOperation }} {{ secondNum }}
        <span v-if="typeof result === 'number'">=</span>
      </p>

      <input v-model="result" placeholder="0" />

      <button @click="input(num)" class="cell num" v-for="num in numbers">
        {{ num }}
      </button>

      <button @click="reset()" class="cell op">C</button>
      <button @click="clear()" class="cell op">Del</button>
      <button @click="input('.')" class="cell op">.</button>
      <button @click="percent()" class="cell op">%</button>

      <button @click="updateOperation(operation)" class="cell op" v-for="operation in operations">
        {{ operation }}
      </button>

      <button @click="calc()" class="cell op">=</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Calculator',
  data() {
    return {
      calculated: false,
      firstNum: '',
      secondNum: '',
      currentNumber: 'firstNum',
      currentOperation: null,
      result: '',
      clearResult: '',
      numbers: [1, 2, 3, 4, 5, 6, 7, 8, 9, 0],
      operations: ['+', '-', '/', '*'],
    }
  },
  methods: {
    input: function (char) {
      if (this.calculated) {
        this.reset()
        this.calculated = false
      }

      if (
        (char === '.' && (this[this.currentNumber].includes('.') || !this[this.currentNumber])) ||
        (char === 0 && !this[this.currentNumber])
      ) {
        return
      }

      this[this.currentNumber] += char
      this.result = this[this.currentNumber]
    },
    reset: function () {
      this.firstNum = ''
      this.secondNum = ''
      this.currentNumber = 'firstNum'
      this.currentOperation = null
      this.result = ''
      this.clearResult = ''
    },
    calc: function () {
      if (this.currentOperation) {
        this.result = eval(`${this.firstNum}${this.currentOperation}${this.secondNum}`)
        this.calculated = true
      }
    },
    updateOperation: function (operation) {
      if (this.firstNum) {
        this.currentOperation = operation
        this.currentNumber = 'secondNum'
      }
    },
    clear: function () {
      if (this.currentOperation) {
        this.secondNum = this.secondNum.slice(0, -1)
        this.result = this.secondNum
      } else {
        this.firstNum = this.firstNum.slice(0, -1)
        this.result = this.firstNum
      }
    },
    percent: function () {
      this.result = this.result / 100
    },
  },
}
</script>
