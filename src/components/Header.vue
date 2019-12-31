<template>
  <div>
  <b-nav tabs :totalCorrectAnswers="totalCorrectAnswers">
      <b-nav-item disabled><b>Quiz App</b></b-nav-item>
      <b-nav-item disabled>
        Correct Answers: {{ totalCorrectAnswers }}/{{ totalQuestions }}
      </b-nav-item>
  </b-nav>
    </div>
</template>

<script>
  export default {
    props: [
      'totalCorrectAnswers',
      'totalQuestions',
    ]
  }
</script>