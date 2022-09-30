# Trivia Quiz game
In the pursuit of learning Vue JS, following along with a course on Udemy, this project was made successfully by the instructor of the Udemy course that I enrolled. Highly thankful and grateful for being able to complete this small little project while learning Vue JS. This uses localStorage to store the score board.

### Link to the site:- [quize-game](https://quize-game-amber.vercel.app/)

# How to run
1. clone the repo and `cd` into the project folder
2. run `npm install` and `npm run dev` to start the server

# Here's what I learnt
* I lerned about how Vue JS works
* Vue Directives
* How to prase HTML data using `v-html` directive
* Data binding with `v-bind` `v-model` and `v-click`
* Looping with `v-for`
* Condition rendering with `v-if` and `v-else`
* Learnt properties like `computed`, `methods`, `props`, `data`, `components`
* Composition API along with some Options API (I prefer Composition API vtw)
* Life Cycle Hooks
* Passing values to Child Components using props
* Two Way Binding

### Code Snippet
The below code snippet has helped me understand and apply the logical reason and approach to a problem.

```javascript
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
```