<template>
  <div id="app">
    <Header 
      :numCorrect="numCorrect"
      :total="total"
    />
    <b-container>
      <b-row>
        <b-col sm="6" offset="3">
          <h1>Question {{ index+1 }} of {{ questions.length }}</h1>
          <QuestionBox
            v-if="questions.length > 0"
            :current-question="questions[index]"
            :next="next"
            :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'

export default {
  name: 'App',
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      total: 0
    }
  },
  methods: {
    next: function() {
      this.index++;
    },
    increment(isCorrect) {
      if(isCorrect) {
        this.numCorrect++
      }
      this.total++
    }
  },
  mounted: function() {
    fetch('https://opentdb.com/api.php?amount=10&category=15&type=multiple', {
      method: 'get'
    })
      .then((response) => {
        return response.json()
      })
      .then((jsonData) => {
        this.questions = jsonData.results;
      })
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
  margin-top: 60px;
}
</style>
