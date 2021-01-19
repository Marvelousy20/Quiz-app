<template>
  <Header 
    :totalNum = "totalNum"
    :correctNum = "correctNum"
  />
  <Quizsec 
    v-if="questions.length"
    :currentQuestion = "questions[index]"
    :next = "next"
    :increment="increment"
  />
</template>

<script>
import Header from './components/Header.vue'
import Quizsec from './components/Quizsec.vue'

export default {
  name: 'App',
  components: {
    Header, 
    Quizsec
  },
  data() {
    return {
      questions: [],
      index: 0,
      totalNum: 0,
      correctNum: 0,
    }
  },
  mounted: function() {
    fetch('https://opentdb.com/api.php?amount=10&category=21&type=multiple')
    .then(response => {
      return response.json()
    })
    .then(responseData => {
      this.questions = responseData.results ;
      console.log(responseData)
    })
    .catch(err => console.log(err))
  },
  methods: {
    next() {
      this.index++
    },
  
    // Keep track of total number of questions answered and total number of correct answers
    increment(isCorrect) {
      if(isCorrect) {
        this.correctNum++
      }

      this.totalNum++
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
