<template>
  <div class="calculator">
    <div>
      <input type="text" v-model="result" class="console" readonly>
    </div>
    <ul>
      <li>
        <button type="button" class="action" @click="reset()">AC</button>
        <button type="button" class="action" @click="changeSign()">+/-</button>
        <button type="button" class="action" @click="calculatePercentage()">%</button>
        <button type="button" class="operation" v-bind:class="{ highlighted: highlighted === '/' }" @click="createOperation('/')">/</button>
      </li>
      <li>
        <button type="button" @click="number('7')">7</button>
        <button type="button" @click="number('8')">8</button>
        <button type="button" @click="number('9')">9</button>
        <button type="button" class="operation" v-bind:class="{ highlighted: highlighted === 'X' }" @click="createOperation('X')">X</button>
      </li>
      <li>
        <button type="button" @click="number('4')">4</button>
        <button type="button" @click="number('5')">5</button>
        <button type="button" @click="number('6')">6</button>
        <button type="button" class="operation" v-bind:class="{ highlighted: highlighted === '-' }" @click="createOperation('-')">-</button>
      </li>
      <li>
        <button type="button" @click="number('1')">1</button>
        <button type="button" @click="number('2')">2</button>
        <button type="button" @click="number('3')">3</button>
        <button type="button" class="operation" v-bind:class="{ highlighted: highlighted === '+' }" @click="createOperation('+')">+</button>
      </li>
      <li>
        <button type="button" class="zero" @click="number('0')">0</button>
        <button type="button" @click="convertFloat()">.</button>
        <button type="button" class="operation"  @click="makeOperation()">=</button>
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  data () {
    return {
      result: 0,
      firstElement: null,
      secondElement: null,
      operator: null,
      highlighted: null
    }
  },
  methods: {
    reset () {
      this.result = 0
      this.firstElement = null
      this.secondElement = null
      this.operator = null
      this.highlighted = null
    },
    number (data) {
      this.highlighted = null
      if (this.result) {
        this.result += data
      } else {
        this.result = data
      }
      if (this.firstElement) {
        if (this.secondElement) {
          this.secondElement += data
          this.result = this.secondElement
        } else {
          this.secondElement = data
          this.result = data
        }
      }
    },
    changeSign () {
      this.result = this.result * -1
    },
    convertFloat () {
      if (!String(this.result).includes('.')) {
        if (this.firstElement) {
          if (this.secondElement) {
            this.result = this.result + '.'
            this.secondElement += '.'
          } else {
            this.result = '0.'
          }
        } else {
          this.result = this.result + '.'
        }
      }
    },
    calculatePercentage () {
      if (this.result && !this.secondElement) {
        this.result = this.result / 100
        this.firstElement = this.result
      }
      if (this.firstElement && this.secondElement) {
        this.secondElement = this.firstElement * this.secondElement / 100
        this.result = this.secondElement
      }
    },
    createOperation (operator) {
      if (this.highlighted) {
        this.operator = operator
        this.highlighted = operator
      } else {
        this.highlighted = operator
        if (this.firstElement && this.secondElement) {
          this.makeOperation()
          this.firstElement = this.result
          this.secondElement = null
        } else {
          this.firstElement = this.result
        }
        this.operator = operator
      }
    },
    makeOperation () {
      switch (this.operator) {
        case '+':
          console.log(typeof this.firstElement)
          this.result = (String(this.firstElement).includes('.') ? parseFloat(this.firstElement) : parseInt(this.firstElement)) + (String(this.secondElement).includes('.') ? parseFloat(this.secondElement) : parseInt(this.secondElement))
          break
        case '-':
          this.result = (String(this.firstElement).includes('.') ? parseFloat(this.firstElement) : parseInt(this.firstElement)) - (String(this.secondElement).includes('.') ? parseFloat(this.secondElement) : parseInt(this.secondElement))
          break
        case 'X':
          this.result = (String(this.firstElement).includes('.') ? parseFloat(this.firstElement) : parseInt(this.firstElement)) * (String(this.secondElement).includes('.') ? parseFloat(this.secondElement) : parseInt(this.secondElement))
          break
        case '/':
          this.result = (String(this.firstElement).includes('.') ? parseFloat(this.firstElement) : parseInt(this.firstElement)) / (String(this.secondElement).includes('.') ? parseFloat(this.secondElement) : parseInt(this.secondElement))
          break
      }
      this.secondElement = null
    }
  }
}
</script>
<style lang="scss">
.calculator {
  width: 300px;
  padding: 30px 10px 10px 10px;
  border-radius: 15px;
  background: #000;
  margin: 100px auto 0 auto;
  input {
    border: 0 none;
    background: #000;
    color: #fff;
    font-size: 50px;
    width: 92%;
    text-align: right;
    margin-bottom: 20px;
    overflow: hidden;
    font-family: monospace;
  }
  ul {
    margin:0;
    padding: 0;
    li {
      list-style-type: none;
      button {
        border: 0 none;
        background: #333;
        color: #fff;
        font-size: 20px;
        border-radius: 100%;
        height: 60px;
        width:60px;
        line-height: 60px;
        margin: 7px;
        cursor: pointer;
        &.action {
          background: #a5a5a5;
        }
        &.operation {
          background: #f1a43c;
          &.highlighted {
            color: #f1a43c;
            background: #fff;
          }
        }
        &.zero {
          width: 138px;
          border-radius: 30px;
        }
      }
    }
  }
}

</style>
