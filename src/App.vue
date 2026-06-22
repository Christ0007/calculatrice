<script setup>
import { ref } from 'vue'

const expression = ref('')

function ajouter(valeur) {
  expression.value += valeur
}

function effacer() {
  expression.value = ''
}

function calculer() {
  try {
    expression.value = eval(expression.value)
  } catch {
    expression.value = 'Erreur'
  }
}
</script>

<template>
  <div class="calculator">
    <h1>Calculatrice</h1>

    <input
      type="text"
      :value="expression"
      readonly
      class="display"
    >

    <div class="buttons">
      <button @click="effacer">C</button>
      <button @click="ajouter('/')">/</button>
      <button @click="ajouter('*')">*</button>
      <button @click="ajouter('-')">-</button>

      <button @click="ajouter('7')">7</button>
      <button @click="ajouter('8')">8</button>
      <button @click="ajouter('9')">9</button>
      <button @click="ajouter('+')">+</button>

      <button @click="ajouter('4')">4</button>
      <button @click="ajouter('5')">5</button>
      <button @click="ajouter('6')">6</button>
      <button @click="calculer">=</button>

      <button @click="ajouter('1')">1</button>
      <button @click="ajouter('2')">2</button>
      <button @click="ajouter('3')">3</button>
      <button @click="ajouter('0')">0</button>
    </div>
  </div>
</template>

<style>
.calculator {
  max-width: 350px;
  margin: 50px auto;
  text-align: center;
}

.display {
  width: 100%;
  padding: 15px;
  font-size: 24px;
  margin-bottom: 15px;
}

.buttons {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 10px;
}

button {
  padding: 15px;
  font-size: 20px;
  cursor: pointer;
}
</style>