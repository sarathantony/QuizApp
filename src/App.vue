<template>
  <div id="app">
    <Header
      :totalCorrectAnswers="totalCorrectAnswers"
      :totalQuestions="totalQuestions"
    />
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :nextQuestion="nextQuestion"
            :totalQuestions="totalQuestions"
            :totalCorrectAnswers="totalCorrectAnswers"
            :trackAnswers="trackAnswers"
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
  name: 'app',
  components: {
    Header,
    QuestionBox,
  },
  data() {
    return {
      questions: [],
      index: 0,
      totalQuestions: 0,
      totalCorrectAnswers: 0,
    }
  },
  methods: {
    nextQuestion() {
      this.index ++
    },
    trackAnswers(isCorrect) {
      if (isCorrect) this.totalCorrectAnswers++

      this.totalQuestions++
    }
  },
  mounted: function() {
    fetch('https://opentdb.com/api.php?amount=10&category=28&type=multiple', {
      method: 'get'
    })
    .then(response => response.json())
    .then(data => this.questions = data.results)
  },
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
