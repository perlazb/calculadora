<script setup lang="ts"></script>

<template>
  <div class="calculator">
    <div class="display">{{ display }}</div>
    <div class="button-row">
      <button class="button" v-on:click="appendNumber('1')">1</button>
      <button class="button" v-on:click="appendNumber('2')">2</button>
      <button class="button" v-on:click="appendNumber('3')">3</button>
      <button class="button operator" v-on:click="chooseOperation('+')">+</button>
    </div>
    <div class="button-row">
      <button class="button" v-on:click="appendNumber('4')">4</button>
      <button class="button" v-on:click="appendNumber('5')">5</button>
      <button class="button" v-on:click="appendNumber('6')">6</button>
      <button class="button operator" v-on:click="chooseOperation('-')">-</button>
    </div>
    <div class="button-row">
      <button class="button" v-on:click="appendNumber('7')">7</button>
      <button class="button" v-on:click="appendNumber('8')">8</button>
      <button class="button" v-on:click="appendNumber('9')">9</button>
      <button class="button operator" v-on:click="chooseOperation('*')">*</button>
    </div>
    <div class="button-row">
      <button class="button zero" v-on:click="appendNumber('0')">0</button>
      <button class="button operator" v-on:click="chooseOperation('/')">/</button>
      <button class="button operator" v-on:click="compute()">=</button>
      <button class="button operator" v-on:click="clear()">C</button>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'

export default defineComponent({
  name: 'Calculator',
  setup() {
    const display = ref('')
    const currentOperand = ref('')
    const previousOperand = ref('')
    const operation = ref<string | null>(null)

    const clear = () => {
      display.value = ''
      currentOperand.value = ''
      previousOperand.value = ''
      operation.value = null
    }

    const appendNumber = (number: string) => {
      if (number === '.' && currentOperand.value.includes('.')) return
      currentOperand.value += number
      updateDisplay()
    }

    const chooseOperation = (op: string) => {
      if (currentOperand.value === '') return
      if (previousOperand.value !== '') {
        compute()
      }
      operation.value = op
      previousOperand.value = currentOperand.value
      currentOperand.value = ''
    }

    const compute = () => {
      let computation: number
      const prev = parseFloat(previousOperand.value)
      const curr = parseFloat(currentOperand.value)

      if (isNaN(prev) || isNaN(curr)) return

      switch (operation.value) {
        case '+':
          computation = prev + curr
          break
        case '-':
          computation = prev - curr
          break
        case '*':
          computation = prev * curr
          break
        case '/':
          computation = prev / curr
          break
        default:
          return
      }

      currentOperand.value = computation.toString()
      operation.value = null
      previousOperand.value = ''
      updateDisplay()
    }

    const updateDisplay = () => {
      display.value = currentOperand.value
    }

    return {
      display,
      clear,
      appendNumber,
      chooseOperation,
      compute
    }
  }
})
</script>

<style scoped>
.calculator {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  background: #f9f9f9;
  text-align: center;
}

.display {
  font-size: 2em;
  margin-bottom: 10px;
  padding: 10px;
  background: #222;
  color: #fff;
  border-radius: 5px;
}

.button-row {
  display: flex;
  justify-content: space-between;
  margin-bottom: 15px;
}

.button {
  flex: 1;
  margin: 15px;
  padding: 25px;
  font-size: 1.5em;
  border: none;
  border-radius: 5px;
  background: #6c4b0f;
  color: #fff;
  cursor: pointer;
  transition: background 0.3s;
}

.button:hover {
  background: #be9219;
}

.button.operator {
  background: #000000;
}

.button.operator:hover {
  background: #5c5c5c;
}

.button.zero {
  flex: 2;
}
</style>
