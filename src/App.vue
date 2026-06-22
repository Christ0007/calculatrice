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

    expression.value = resultat.toString()
  } catch {
    expression.value = 'Erreur'
  }
}

function gererClavier(event) {
  const touche = event.key

  const touchesAutorisees = [
    '0','1','2','3','4',
    '5','6','7','8','9',
    '+','-','*','/','.'
  ]

  if (touchesAutorisees.includes(touche)) {
    ajouter(touche)
  }

  if (touche === 'Enter') {
    calculer()
  }

  if (touche === 'Backspace') {
    supprimerDernier()
  }

  if (touche === 'Escape') {
    effacer()
  }
}

onMounted(() => {
  window.addEventListener('keydown', gererClavier)
})

onUnmounted(() => {
  window.removeEventListener('keydown', gererClavier)
})
</script>

<template>
  <div class="min-h-screen bg-slate-100 flex items-center justify-center p-4">
    <div class="bg-white rounded-2xl shadow-lg p-6 w-full max-w-md">

      <h1 class="text-2xl font-bold text-center mb-4">
        Calculatrice Vue.js
      </h1>

      <input
        type="text"
        :value="expression"
        readonly
        class="w-full border rounded-lg p-4 text-right text-2xl mb-4"
      />

      <div class="grid grid-cols-4 gap-2">

        <button @click="effacer" class="btn">C</button>
        <button @click="supprimerDernier" class="btn">⌫</button>
        <button @click="ajouter('/')" class="btn">÷</button>
        <button @click="ajouter('*')" class="btn">×</button>

        <button @click="ajouter('7')" class="btn">7</button>
        <button @click="ajouter('8')" class="btn">8</button>
        <button @click="ajouter('9')" class="btn">9</button>
        <button @click="ajouter('-')" class="btn">−</button>

        <button @click="ajouter('4')" class="btn">4</button>
        <button @click="ajouter('5')" class="btn">5</button>
        <button @click="ajouter('6')" class="btn">6</button>
        <button @click="ajouter('+')" class="btn">+</button>

        <button @click="ajouter('1')" class="btn">1</button>
        <button @click="ajouter('2')" class="btn">2</button>
        <button @click="ajouter('3')" class="btn">3</button>

        <button
          @click="calculer"
          class="row-span-2 bg-blue-500 text-white rounded-lg"
        >
          =
        </button>

        <button
          @click="ajouter('0')"
          class="col-span-2 btn"
        >
          0
        </button>

        <button @click="ajouter('.')" class="btn">
          .
        </button>
      </div>

      <div v-if="historique.length" class="mt-6">
        <h2 class="font-bold mb-2">
          Historique
        </h2>

        <ul class="space-y-1">
          <li
            v-for="(item,index) in historique"
            :key="index"
            class="text-gray-600"
          >
            {{ item }}
          </li>
        </ul>
      </div>

    </div>
  </div>
</template>

<style scoped>
.btn {
  @apply bg-gray-200 rounded-lg p-4 text-xl hover:bg-gray-300;
}
</style>