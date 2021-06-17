<template>
  <div class="container text-center">
    <h1 class="result-header">Ошибка регистрации</h1>
    <br>
    <hr>
    <div class="register-message">
      <p class="font-weight-light">Не удалось зарегистрировать нового пользователя</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "register_failed"
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
