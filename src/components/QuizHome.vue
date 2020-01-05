<template>
  <div>
    <QuizForm
      :catagories="catagories"
      :difficulty="difficulty"
      :type="type"
      :amount="amount"
    />
  </div>
</template>

<script>
  import QuizForm from './QuizForm'

  export default {
    components: {
      QuizForm,
    },
    data() {
      return {
        catagories: [],
        difficulty: [
          {id: 'any', label: 'Any Difficulty'},
          {id: 'eazy', label: 'Eazy'},
          {id: 'medium', label: 'Medium'},
          {id: 'hard', label: 'Hard'},
        ],
        amount: 0,
        type: [
          {id: 'any', label: 'Any'},
          {id: 'multiple', label: 'Multiple'},
          {id: 'boolean', label: 'True/False'},
        ]
      }
    },
    mounted: function() {
      // CATEGORIES
      fetch('https://opentdb.com/api_category.php', {
        method: 'get'
      })
      .then(response => response.json())
      .then(data => this.catagories = data.trivia_categories)
    }
    
  }
</script>