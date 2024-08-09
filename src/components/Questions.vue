<script>
import axios from "axios"

export default {
  data() {
    return {
      questions: [],
      currentQuestion: {},
      showAnswer: false,
      timer: 10, // Countdown timer
      interval: null // To store the interval ID
    }
  },
  mounted() {
    axios
      .get("/questions.json")
      .then((result) => (this.questions = result.data))

    this.currentQuestion = this.questions[0]
  },
  methods: {
    async getRandomQuestion() {
      this.showAnswer = false
      this.timer = 10 // Reset the timer
      const randomIndex = Math.floor(Math.random() * this.questions.length)
      this.currentQuestion = this.questions[randomIndex]
      clearInterval(this.interval)
      this.interval = setInterval(async () => {
        if (this.timer > 0) {
          this.timer--
        } else {
          clearInterval(this.interval)
        }
      }, 1000)
    },
    showAnswerNow() {
      this.showAnswer = true
    }
  },
  beforeDestroy() {
    clearInterval(this.interval) // Clear the interval when the component is destroyed
  }
}
</script>
<template>
  <div class="row row-cols-1 mx-auto">
    <div
      class="col d-flex flex-column align-items-center justify-content-center"
    >
      <div class="">
        <div>
          <h1 id="timer">{{ timer }}</h1>
        </div>
        <div>
          <h2 id="question">{{ currentQuestion.question }}</h2>
        </div>
        <div v-if="showAnswer" id="ans">
          <hr />
          <h2 class="text-danger">{{ currentQuestion.answer }}</h2>
        </div>
      </div>
      <hr />
    </div>
    <div
      class="col d-flex flex-column align-items-center justify-content-center"
    >
      <div class="" style="max-width: 300px">
        <button @click="getRandomQuestion()" class="btn btn-primary mb-3">
          Get Random Question
        </button>
        <button
          @click="
            () => {
              showAnswer = true
            }
          "
          class="btn btn-outline-primary"
        >
          Show Answer
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
#timer {
  color: black;
  font-size: 96px;
}

#question {
  font-size: 48px;
}
</style>
