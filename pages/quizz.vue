<template>
<div class="container justify-content-center">
<!--    <p class="row">{{ quizzData.questions }}</p>-->
  <div class="d-flex justify-content-center">
    <h2 class="row text-center"> {{quizzData.name}} </h2>
  </div>
  <div class="counter text-center">
    <counter />
  </div>
  <br>
  <b-form class="quizz-form container" @submit.prevent="sendQuizzData">
    <div>
      <question-card class="question-card"
                     v-for="(questionObject, index) in quizzData.questions.slice(0, 1)"
                     :key="index"
                     :quizzQuestion="questionObject"
                     :question-type="'singleSelect'"
                     :questionIndex="1"
                     @singleSelectResult="getSingleSelectResult"
      >
      </question-card>

      <question-card class="question-card"
                     v-for="(questionObject, index) in quizzData.questions.slice(1, 2)"
                     :key="index"
                     :quizzQuestion="questionObject"
                     :question-type="'multipleSelectDualbox'"
                     :questionIndex="2"
                     @multiSelectDualboxResult="getMultiSelectDualboxResult"
      >
      </question-card>

      <question-card class="question-card"
                     v-for="(questionObject, index) in quizzData.questions.slice(2, 3)"
                     :key="index"
                     :quizzQuestion="questionObject"
                     :question-type="'singleSelect'"
                     :questionIndex="3"
                     @singleSelectResult="getSingleSelectResult"
      >
      </question-card>

      <question-card class="question-card"
                     v-for="(questionObject, index) in quizzData.questions.slice(3, 4)"
                     :key="index"
                     :quizzQuestion="questionObject"
                     :question-type="'dragAndDrop'"
                     input-class="container"
                     :questionIndex="4"
                     @dragAndDropSelectResult="getDragAndDropSelectResult"
      >
      </question-card>

      <question-card class="question-card"
                     v-for="(questionObject, index) in quizzData.questions.slice(4, 5)"
                     :key="index"
                     :quizzQuestion="questionObject"
                     :question-type="'multipleSelect'"
                     :questionIndex="5"
                     @multipleSelectResult="getMultipleSelectResult"
      >
      </question-card>

      <question-card class="question-card"
                     v-for="(questionObject, index) in quizzData.questions.slice(5, 6)"
                     :key="index"
                     :quizzQuestion="questionObject"
                     :question-type="'singleSelect'"
                     :questionIndex="6"
                     @singleSelectResult="getSingleSelectResult"
      >
      </question-card>

      <question-card class="question-card"
                     v-for="(questionObject, index) in quizzData.questions.slice(6, 7)"
                     :key="index"
                     :quizzQuestion="questionObject"
                     :question-type="'singleSelect'"
                     :questionIndex="7"
                     @singleSelectResult="getSingleSelectResult"
      >
      </question-card>

      <question-card class="question-card"
                     v-for="(questionObject, index) in quizzData.questions.slice(7, 8)"
                     :key="index"
                     :quizzQuestion="questionObject"
                     :question-type="'multipleSelectDualbox'"
                     :questionIndex="8"
                     @multiSelectDualboxResult="getMultiSelectDualboxResult"
      >
      </question-card>

      <question-card class="question-card"
                     v-for="(questionObject, index) in quizzData.questions.slice(8, 9)"
                     :key="index"
                     :quizzQuestion="questionObject"
                     :question-type="'singleSelect'"
                     :questionIndex="9"
                     @singleSelectResult="getSingleSelectResult"
      >
      </question-card>

      <question-card class="question-card"
                     v-for="(questionObject, index) in quizzData.questions.slice(9, 10)"
                     :key="index"
                     :quizzQuestion="questionObject"
                     :question-type="'dragAndDrop'"
                     input-class="container"
                     :questionIndex="10"
                     @dragAndDropSelectResult="getDragAndDropSelectResult"
      >
      </question-card>

      <div class="row justify-content-center">
        <b-button type="submit" variant="success">Отправить</b-button>
      </div>
    </div>
  </b-form>


</div>
</template>

<script>
import QuestionCard from "~/components/quizz/questionCard";
import Counter from "~/components/quizz/counter";

export default {
  name: "quizz",
  components: {Counter, QuestionCard },
  layout: 'quizzLayout',
  data() {
    return {
      quizzData: {'questions': ''},
      quizzName: '',
      quizzDescription: '',
      userAnswers: {_id: null, questions: []}
    }
  },

  methods: {
    // Initial quizz data loading
    async getQuizzData() {
      const response = await fetch('http://217.160.214.250:3000/api/v1/quizz/60bf34d20472f3440739b588', {
        method: 'GET',
        credentials: "include"
      })
      this.quizzData = await response.json()
    },

    // Normalize user answers for JSON POST-request
    async prepareQuizzData(userAnswers, questionId, questionAnswersIdList) {
      userAnswers._id = this.quizzData._id

      const newAnswer = {_id: questionId, answers: questionAnswersIdList}

      if (userAnswers.questions.length === 0) {
        userAnswers.questions.push(newAnswer);
      } else {
        for (let i = 0; i < userAnswers.questions.length; i++) {
          let answerObject = userAnswers.questions[i]
          if (answerObject._id === newAnswer._id) {
            answerObject.answers = newAnswer.answers
            break
          } else if (answerObject !== newAnswer._id && i === userAnswers.questions.length - 1) {
            userAnswers.questions.push(newAnswer)
            break
          }
        }
      }
    },

    // Get data from child elements block
    async getSingleSelectResult(data) {
      let answerIdListView = []
      answerIdListView.push(data.answerDetails.answerObject)

      data.answerDetails.answerObject = answerIdListView

      await this.prepareQuizzData(this.userAnswers, data.answerDetails.questionId, data.answerDetails.answerObjectsList)
    },

    async getMultipleSelectResult(data) {
      await this.prepareQuizzData(this.userAnswers, data.answerDetails.questionId, data.answerDetails.answerObjectsList)
    },

    async getDragAndDropSelectResult(data) {

      await this.prepareQuizzData(this.userAnswers, data.answerDetails.questionId, data.answerDetails.answerObjectsList)
    },

    async getMultiSelectDualboxResult(data) {
      await this.prepareQuizzData(this.userAnswers, data.answerDetails.questionId, data.answerDetails.answerObjectsList)
    },

    async sendQuizzData() {
      const response = await fetch('http://217.160.214.250:3000/api/v1/quizz/60bf34d20472f3440739b588', {
        method: 'POST',
        headers: {'Content-Type': 'application/json'},
        body: JSON.stringify(this.userAnswers),
        credentials: 'include'
      });

      const jsonResponse = await response.json()
      if (jsonResponse.AttemptsError) {
        await this.$router.push('/attempts_limit')
      } else {
        await this.$router.push('/result')
      }
    }
  },

  mounted() {
    this.getQuizzData()
  }

}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Catamaran');

.container {
  padding: 2em;
}

.counter {
  margin: auto;
  width: 18%;
}

.quizz-form {
  padding: 2em;
  margin-bottom: 2em;
  border: 1px solid #a8a8a8;
  border-radius: .5em;
  max-width: 700px;
  box-sizing: border-box;
  font-family: 'Catamaran', serif;
}

</style>
