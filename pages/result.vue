<template>
<div class="container text-center">
  <h1 class="result-header">Результат</h1>
  <br>
  <hr>
  <div class="result-details" v-if="quizzResult.mark">
    <p>Ваша оценка: {{ quizzResult.mark}}</p>
    <p>Вы ответили правильно на {{ quizzResult.result * 100}} % вопросов.</p>
  </div>
  <div class="result-error" v-else>
    <p>Истекла сессия для проверки результатов теста</p>
  </div>
</div>
</template>

<script>
export default {
  name: "result",
  data() {
    return {
      quizzResult: {}
    }
  },

  methods: {
    async getQuizzResult() {
      const response = await fetch('http://217.160.214.250:3000/api/v1/quizz/result', {
        method: 'GET',
        credentials: "include"
      })
      this.quizzResult = await response.json()
    }
  },

  mounted() {
    this.getQuizzResult()
  }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Catamaran');

.container {
  font-family: 'Catamaran', sans-serif;
}

.result-header {
  margin-top: 1em;
}

</style>
