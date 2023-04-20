<template>
  <div class="tableau">
    <div>
      <div v-for="(tabs, tabKey) in tableau" :key="tabKey">
        <div v-for="(tab, key) in tabs" :key="key" :data-tab-id="tabKey" :data-id="key" @click="play" class="">{{ tab }}</div>
      </div>
      <a href="#" @click="refresh" class="inline-flex items-center justify-center px-5 py-3 text-base font-medium text-center text-white bg-orange-700 rounded-lg hover:bg-orange-800 focus:ring-4 focus:ring-orange-300 dark:focus:ring-orange-900">
        Réinitialiser
      </a>
    </div>
    <div v-if="result !== 0" class="p-10 text-center">
      <h1 class="mb-4 text-4xl font-extrabold leading-none tracking-tight text-gray-900 md:text-5xl lg:text-6xl dark:text-white">{{ result === 1 ? "Joueur 1 a gagné" : result === 2 ? "Joueur 2 a gagné" : "Aucun joueur n'a gagné"}}</h1>
      <p class="mb-6 text-lg font-normal text-gray-500 lg:text-xl sm:px-16 xl:px-48 dark:text-gray-400">Vous pouvez cliquer sur le bouton ci-dessous pour relancer une partie !</p>
      <a href="#" @click="refresh" class="inline-flex items-center justify-center px-5 py-3 text-base font-medium text-center text-white bg-blue-700 rounded-lg hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 dark:focus:ring-blue-900">
        Relancer une partie
        <svg class="w-5 h-5 ml-2 -mr-1" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M10.293 3.293a1 1 0 011.414 0l6 6a1 1 0 010 1.414l-6 6a1 1 0 01-1.414-1.414L14.586 11H3a1 1 0 110-2h11.586l-4.293-4.293a1 1 0 010-1.414z" clip-rule="evenodd"></path></svg>
      </a>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  name: 'IndexPage',
  data() {
    return {
      tableau: [
        ["", "", ""],
        ["", "", ""],
        ["", "", ""],
      ],
      j1: Boolean,
      j2: Boolean,
      result: 0,
      tour: 0
    }
  },
  methods: {
    play(event) {
      console.log("click")
      // Si c'est le tour du joueur 1, on met un X si la case est vide
      // On vérifie si la case est vide
      console.log("result", this.result)
      if (this.result === 0) {
        if (this.tableau[event.target.dataset.tabId][event.target.dataset.id] === "") {
          if (this.tour%2 === 0) {
            this.tableau[event.target.dataset.tabId][event.target.dataset.id] = "X"
          } else {
            this.tableau[event.target.dataset.tabId][event.target.dataset.id] = "O"
          }
          this.tour += 1;
          // On force le re-render de la page dans ça ne met pas à jour le DOM
          this.$forceUpdate()
          this.checkWin();
        }
      } else {
        alert("Jeu terminé")
      }
    },
    checkWin() {
      // On vérifie si dans chaque ligne, il y a 3 X ou 3 O
      this.tableau.forEach((tab) => {
        if (tab[0] === tab[1] && tab[1] === tab[2] && tab[0] !== "") {
          if (tab[0] === "X") {
            this.result = 1
          } else if (tab[0] === "O") {
            this.result = 2
          }
        }
      })
      // On vérifie si dans chaque colonne, il y a 3 X ou 3 O
      for (let i = 0; i < 3; i++) {
        if (this.tableau[0][i] === this.tableau[1][i] && this.tableau[1][i] === this.tableau[2][i] && this.tableau[0][i] !== "") {
          if (this.tableau[0][i] === "X") {
            this.result = 1
          } else if (this.tableau[0][i] === "O") {
            this.result = 2
          }
        }
      }
      // On vérifie les diagonales
      console.log("diagonale", this.tableau[0][0], this.tableau[1][1], this.tableau[2][2])
      if (
        (this.tableau[0][0] === this.tableau[1][1] && this.tableau[1][1] === this.tableau[2][2] && this.tableau[0][0] !== "") ||
        (this.tableau[2][0] === this.tableau[1][1] && this.tableau[1][1] === this.tableau[0][2] && this.tableau[2][0] !== "")) {
        console.log("WIN")
        if (this.tableau[0][0] === "X" || this.tableau[2][0] === "X") {
          console.log("WIN J1")
          this.result = 1
        } else if (this.tableau[0][0] === "O" || this.tableau[2][0] === "O") {
          console.log("WIN J2")
          this.result = 2
        }
      }
      // On vérifie si le tableau est plein
      if (this.tableau[0].includes("") === false && this.tableau[1].includes("") === false && this.tableau[2].includes("") === false) {
        this.result = 3
      }
    },
    refresh(event) {
      event.preventDefault();
      this.tableau = [
        ["", "", ""],
        ["", "", ""],
        ["", "", ""],
      ]
      this.result = 0
      this.tour = 0
      this.$forceUpdate();
    }
  }
})
</script>

<style>
.tableau a {
  margin-top: 2rem;
  width: 100%;
}
.tableau {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 1rem;
}
.tableau > div > div {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  background: bisque;
  height: 20vh;
  width: 30vw;
}
.tableau > div > div > div {
  cursor: pointer;
  border: 2px solid #000;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 5rem;
}
</style>
