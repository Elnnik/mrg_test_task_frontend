<template>
  <div class="container text-center">
    <h1 class="result-header">Ошибка входа в систему</h1>
    <br>
    <hr>
    <div class="login-message">
      <p class="font-weight-light">Не удалось войти в систему</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "login_failed"
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
