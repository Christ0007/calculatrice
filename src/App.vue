<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const expression = ref('')
const historique = ref([])

function ajouter(valeur) {
  expression.value += valeur
}

function effacer() {
  expression.value = ''
}

function supprimerDernier() {
  expression.value = expression.value.slice(0, -1)
}

function calculer() {
  if (!expression.value) return

  try {
    const resultat = eval(expression.value)

    historique.value.unshift(
      `${expression.value} = ${resultat}`
    )

    expression.value = String(resultat)
  } catch {
    expression.value = 'Erreur'
  }
}

function gererClavier(event) {
  const touche = event.key

  const autorise = [
    '0','1','2','3','4','5','6','7','8','9',
    '+','-','*','/','.'
  ]

  if (autorise.includes(touche)) {
    ajouter(touche)
  }

  if (touche === 'Enter') calculer()
  if (touche === 'Backspace') supprimerDernier()
  if (touche === 'Escape') effacer()
}

onMounted(() => {
  window.addEventListener('keydown', gererClavier)
})

onUnmounted(() => {
  window.removeEventListener('keydown', gererClavier)
})
</script>

<template>
  <div class="calculator">
    <h1>Calculatrice Vue.js</h1>

    <input
      class="display"
      type="text"
      :value="expression"
      readonly
    />

    <div class="buttons">
      <button @click="effacer">C</button>
      <button @click="supprimerDernier">⌫</button>
      <button @click="ajouter('/')">/</button>
      <button @click="ajouter('*')">*</button>

      <button @click="ajouter('7')">7</button>
      <button @click="ajouter('8')">8</button>
      <button @click="ajouter('9')">9</button>
      <button @click="ajouter('-')">-</button>

      <button @click="ajouter('4')">4</button>
      <button @click="ajouter('5')">5</button>
      <button @click="ajouter('6')">6</button>
      <button @click="ajouter('+')">+</button>

      <button @click="ajouter('1')">1</button>
      <button @click="ajouter('2')">2</button>
      <button @click="ajouter('3')">3</button>
      <button @click="calculer">=</button>

      <button @click="ajouter('0')">0</button>
      <button @click="ajouter('.')">.</button>
    </div>

    <div v-if="historique.length" class="history">
      <h2>Historique</h2>
      <ul>
        <li v-for="(item, index) in historique" :key="index">
          {{ item }}
        </li>
      </ul>
    </div>
  </div>
</template>

<style>
.calculator {
  max-width: 350px;
  margin: 50px auto;
  text-align: center;
  font-family: Arial, sans-serif;
}

.display {
  width: 100%;
  padding: 15px;
  font-size: 24px;
  margin-bottom: 15px;
  text-align: right;
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
  border: 1px solid #ccc;
  background: #f5f5f5;
  transition: 0.2s;
}

button:hover {
  background: #e0e0e0;
}

.history {
  margin-top: 20px;
  text-align: left;
}

.history h2 {
  font-size: 18px;
}

.history ul {
  list-style: none;
  padding: 0;
}

.history li {
  font-size: 14px;
  border-bottom: 1px solid #ddd;
  padding: 5px 0;
}
</style>