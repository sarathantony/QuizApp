<template>
  <div class="questions-box">
    <b-jumbotron>
      <template v-slot:lead>
        <span v-html="currentQuestion.question"></span>
      </template>

      <hr class="my-4">
      <b-list-group>
        <b-list-group-item
          v-for="(item, index) in answers"
          :key="index"
          @click.prevent="selectedAnswer(index)"
          :class="setStatus(index)"
        >
          <span v-html="item"></span>
        </b-list-group-item>
      </b-list-group>

      <b-button
        variant="primary"
        @click.prevent="submitAnswer"
        :class="[selectedIndex ? '' : 'btn-disabled']"
        :disabled="selectedIndex === null || submitted"
      >
        Submit
      </b-button>

      <b-button
        variant="success"
        @click.prevent="nextQuestion"
      >
        Next
      </b-button>
    </b-jumbotron>
  </div>
</template>

<script>
  import _ from 'lodash'

  export default {
    props: {
      currentQuestion: Object,
      nextQuestion: Function,
      trackAnswers: Function,
    },
    data() {
      return {
        selectedIndex: null,
        correctIndex: null,
        submitted: false,
      }
    },
    methods: {
      selectedAnswer(index) {
        this.selectedIndex = index
      },
      shuffleAnswers() {
        const { currentQuestion, answers } = this
        this.correctIndex = answers.indexOf(currentQuestion.correct_answer)

        return answers
      },
      submitAnswer() {
        const { selectedIndex, correctIndex, trackAnswers } = this

        trackAnswers(selectedIndex === correctIndex)
        this.submitted = true
      },
      setStatus(index) {
        const { selectedIndex, correctIndex, submitted } = this

        if (submitted) {
          if (index === correctIndex) return 'correct-answer'
          else if (index === selectedIndex && index !== correctIndex) return 'wrong-answer'
        } else return ''
      }
    },
    watch: {
      currentQuestion: {
        immediate: true,
        handler() {
          this.selectedIndex = null
          this.shuffleAnswers()
          this.submitted = false
        },
      },
    },
    computed: {
      answers() {
        const answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
        return _.shuffle(answers)
      }
    },
    updated() {
      // console.log('set status is ', this.setStatus())
    },
  }
</script>

<style scoped>
  .questions-box {
    margin: 40px;
  }
  .list-group {
    margin-bottom: 2rem;
  }
  .list-group-item:hover {
    background: #EEE;
    cursor: pointer;
  }
  .btn {
    margin: 0 0.5rem;
  }
  .selectedIndex {
    background-color: lightblue;
  }
  .correct {
    background-color: lightgreen;
  }
  .incorrect {
    background-color: lightred;
  }
  .btn-disabled {
    cursor: not-allowed;
  }
  .correct-answer {
    background-color: green;
  }
  .wrong-answer {
    background-color: red;
  }
</style>