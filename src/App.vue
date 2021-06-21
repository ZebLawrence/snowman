<template>
  <div id="app">
    <Navbar
      :sound="sound"
      :color="color"
      :difficulty="difficulty"
      @reset="handleGameReset"
      @colorUpdate="handleColorUpdate"
      @difficultyUpdate="handleDifficultyUpdate"
      @phraseUpdate="handlePhraseUpdate"
      @soundUpdate="handleSound"
    />
    <GameBoard
      :sound="sound"
      :color="color"
      :difficulty="difficulty"
      :phrase="activePhrase"
      :guesses="guesses"
      @letterSelect="handleLetterSelect"
    />
  </div>
</template>

<script>
import GameBoard from '@/components/GameBoard'
import Navbar from '@/components/Navbar'

export default {
  name: 'App',
  components: {
    GameBoard,
    Navbar
  },
  data () {
    return {
      activePhrase: 'Snowman',
      color: localStorage.getItem('selectedColor') || '#000',
      difficulty: localStorage.getItem('selectedDifficulty') || 'easy',
      guesses: [],
      sound: false
    }
  },
  methods: {
    handleColorUpdate (color) {
      localStorage.setItem('selectedColor', color)
      this.color = color
    },
    handleDifficultyUpdate (difficulty) {
      localStorage.setItem('selectedDifficulty', difficulty)
      this.difficulty = difficulty
      this.handleGameReset()
    },
    handleGameReset () {
      this.guesses = []
    },
    handleSound () {
      this.sound = !this.sound
    },
    handleLetterSelect (letter) {
      this.guesses.push(letter)
    },
    handlePhraseUpdate (phrase) {
      this.activePhrase = phrase
      this.handleGameReset()
    }
  }
}
</script>
<style src="../node_modules/bootstrap/dist/css/bootstrap.min.css"  />
<style>
input[type="color"]{
  height: 2em;
  width: 2em;
  vertical-align: middle;
}
</style>