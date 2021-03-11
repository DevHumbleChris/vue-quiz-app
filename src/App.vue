<template>
  <div id="app">
    <Header 
      
    />
    <Homepage 
      v-if="questions.length"
      :userRequest="userRequest"
      :currentQuiz="questions[index]"
      :nextQuiz="nextQuiz"
      :index="index"
      :increment="increment"
      :numTotal="numTotal"
      :numCorrect="numCorrect"
    />
  </div>
</template>

<script>
import Header from './components/Header.vue'
import Homepage from './components/Homepage.vue'

export default {
  name: 'App',
  components: {
    Header,
    Homepage
  },
  data(){
    return {
      index: 0,
      questions: [],
      API_URL: "https://opentdb.com/api.php?amount=10&type=multiple",
      openTDB : {
        "any category": `https://opentdb.com/api.php?amount=10&type=multiple`,
        "general knowledge": `https://opentdb.com/api.php?amount=10&category=9&type=multiple`,
        "books": `https://opentdb.com/api.php?amount=10&category=10&type=multiple`,
        "music": `https://opentdb.com/api.php?amount=10&category=12&type=multiple`,
        "video games": `https://opentdb.com/api.php?amount=10&category=15&type=multiple`,
        "science & Nature": `https://opentdb.com/api.php?amount=10&category=17&type=multiple`,
        "science & Computers": `https://opentdb.com/api.php?amount=10&category=18&type=multiple`,
        "science & Mathematics": `https://opentdb.com/api.php?amount=10&category=19&type=multiple`,
        "mythology": `https://opentdb.com/api.php?amount=10&category=20&type=multiple`,
        "sports": `https://opentdb.com/api.php?amount=10&category=21&type=multiple`,
        "history": `https://opentdb.com/api.php?amount=10&category=23&type=multiple`,
        "art": `https://opentdb.com/api.php?amount=10&category=25&type=multiple`,
        "politics": `https://opentdb.com/api.php?amount=10&category=24&type=multiple`,
        "celebrities": `https://opentdb.com/api.php?amount=10&category=26&type=multiple`,
        "animals": `https://opentdb.com/api.php?amount=10&category=27&type=multiple`,
        "vehicles": `https://opentdb.com/api.php?amount=10&category=28&type=multiple`,
        "science & Gadgets": `https://opentdb.com/api.php?amount=10&category=30&type=multiple`,
        "cartoon & Animations": `https://opentdb.com/api.php?amount=10&category=32&type=multiple`
      },
      numTotal: 0,
      numCorrect: 0
    }
  },
  watch: {
    API_URL(){
      this.getQuestions()
      this.index = 0
    }
  },
  methods: {
    userRequest(selected){
      this.API_URL = this.openTDB[selected]
      console.log(this.API_URL)
    },
    getQuestions(){
      fetch(this.API_URL, {
      method: "get"
    }).then( (response) =>{
      return response.json()
    }).then( (jsonData) => {
      this.questions = jsonData.results
    })
    },
    nextQuiz(){
      this.index++
    },
    increment(isCorrect){
      if(isCorrect){
        this.numCorrect++;
      }
      this.numTotal++;
    }
  },
  mounted(){
    this.getQuestions();
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
