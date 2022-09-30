<script>
import ScoreBoard from './components/ScoreBoard.vue'

const api = 'https://opentdb.com/api.php?amount=1&category=9';
export default {
  name: 'App',
  components: {
    ScoreBoard
  },
  data() {
    return {
      question: undefined,
      incorrectAnswers: undefined,
      correctAnswer: undefined,
      chosenAnswer: undefined,
      answerSubmitted: false,
      winCount: 0,
      loseCount: 0,
    }
  },

  computed: {
    answers() {
      var answers = JSON.parse(JSON.stringify(this.incorrectAnswers))
      answers.splice(Math.round(Math.random() * answers.length), 0, this.correctAnswer)
      return answers
    }
  },

  methods: {
    submitAnswer() {
      if(!this.chosenAnswer) {
        alert("Pick an answer")
      }
      else {
        this.answerSubmitted = true
        if(this.chosenAnswer == this.correctAnswer) {
          this.winCount++
          localStorage.setItem("winCount", JSON.stringify(this.winCount))
        }
        else{
          this.loseCount++
          localStorage.setItem("loseCount", JSON.stringify(this.loseCount))
        }
      }
    },

    getNewQuestion() {
      this.answerSubmitted = false
      this.chosenAnswer = undefined
      this.question = undefined

      this.axios.get(api).then((response) => {
      this.question = response.data.results[0].question
      this.incorrectAnswers = response.data.results[0].incorrect_answers
      this.correctAnswer = response.data.results[0].correct_answer
    })
    }
  },

  created() {
    this.getNewQuestion()
    this.winCount = localStorage.getItem("winCount") ? JSON.parse(localStorage.getItem("winCount")) : this.winCount
    this.loseCount = localStorage.getItem("loseCount") ? JSON.parse(localStorage.getItem("loseCount")) : this.loseCount
  }
}

// https://opentdb.com/api.php?amount=1&category=9
</script>

<template>
    <div>

      <ScoreBoard :winCount="this.winCount" :loseCount="this.loseCount" />
      <template v-if="this.question">
      <h1 v-html="this.question"></h1>

      <template v-for="(answer,index) in this.answers" :key="index">
        <input
        :disabled="this.answerSubmitted"
        type="radio" 
        name="options"
        :value="answer"
        v-model="this.chosenAnswer"
        >
        <label v-html="answer"></label><br/>
      </template>

      <button v-if="!this.answerSubmitted" @click="this.submitAnswer()" class="send" type="button">Send</button>

      <section v-if="this.answerSubmitted" class="result">
        <h4 
        v-if="this.chosenAnswer == this.correctAnswer"
        v-html=" 'Congratulations! the answer is ' +  this.correctAnswer + 'is correct.' "
        >
        </h4>
        <h4 v-else
        v-html=" 'Sorry, you picked the wrong answer, the correct answer is '+  this.correctAnswer "
        >
      </h4>
        <button class="send" @click="this.getNewQuestion()" type="button">Next Question</button>
      </section>
      </template>
    </div>
</template>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smooting: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto;
  max-width: 960px;

  input[type=radio] {
    margin: 12px 4px;
  }

  button.send{
    margin-top: 12px;
    height: 40px;
    min-width: 120px;
    padding: 0 16px;
    color: #fff;
    background-color: #1867c0;
    border: 1px solid #1867c0;
    cursor: pointer;
  }
}
</style>
