<script setup>
import { reactive, onMounted, computed } from "vue";
import Vrstica from "./components/Vrstica.vue"
import Tipkovnica from "./components/Tipkovnica.vue"

const state = reactive({
  solution: "sreda",
  guesses : ["", "", "", "", "", ""],
  currentGuessIndex : 0,
  guessedLetters: {
    miss: [],
    found: [],
    hint: [],
  }
})


const wonGame = computed(
() => state.guesses[state.currentGuessIndex-1] == state.solution
);

const lostGame = computed(() => !wonGame.value && state.currentGuessIndex >= 6);

const handleInput = (key) => {
  if(state.currentGuessIndex >= 6 || wonGame.value ) {
    return;
  }
  
  const currentGuess = state.guesses[state.currentGuessIndex];

  if(key == "{enter}"){
    if(currentGuess.length == 5){
      state.currentGuessIndex++;
      for(var i=0; i<= currentGuess.length; i++){
        let c = currentGuess.charAt(i);
        if(c== state.solution.charAt(i)){
          state.guessedLetters.found.push(c);
        } else if(state.solution.indexOf(c) != -1){
          state.guessedLetters.hint.push(c);
        } else {
          state.guessedLetters.miss.push(c);
        }
      }
    }
  } else if(key == "{bksp}") {
    state.guesses[state.currentGuessIndex] = currentGuess.slice(0, -1);
  } else if(currentGuess.length < 5){
    const alphaRegex = /[a-zA-Z]/;
    if(alphaRegex.test(key)){
      state.guesses[state.currentGuessIndex] += key;
    }
  }
};

onMounted(() => {
  window.addEventListener("keyup", (e) => {
    e.preventDefault();
    let key = 
      e.keyCode == 13 ? "{enter}" : e.keyCode == 8 ? "{bksp}" : String.fromCharCode(e.keyCode).toLowerCase();
    handleInput(key);
  });
});

</script>

<template>
  <div class="nav">
    Besedle
  </div>
  <div class="vrstica-wrapper">
    <vrstica v-for="(guess, i) in state.guesses" :key="i" :value="guess" :solution="state.solution"
      :submmited="i < state.currentGuessIndex" />
  </div>
  <p v-if="wonGame" class="zmaga">
    Čestitamo zmagali ste!
  </p>
  <p v-else-if="lostGame" class="poraz">
    Nimate več poskusov!
  </p>
  <tipkovnica @on-key-press="handleInput" :guessedLetters="state.guessedLetters"/>
</template>

<style scoped>
  .vrstica-wrapper {
    margin-top: 300px;
  }
.nav {
  display: flex;
  justify-content: center;
  font-size: 28px;
  padding-top: 12px;
  padding-bottom: 12px;
  font-weight: 700;
  border-bottom: 1px solid rgb(190, 190, 190);
}

.zmaga, .poraz {
  display: flex;
  justify-content: center;
  font-size: 26px;
}
</style>
