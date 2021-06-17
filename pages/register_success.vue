<template>
  <div class="container text-center">
    <h1 class="result-header">Пользователь зарегистрирован успешно</h1>
    <br>
    <hr>
    <div class="register-message">
      <p class="font-weight-light">Регистрация пользователя прошла успешно!</p>
      <p class="font-weight-light">Можете приступить к выполнению Теста, он доступен через вкладку навигационной панели.</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "register_success"
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
