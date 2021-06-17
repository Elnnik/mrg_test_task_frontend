<template>
  <div class="container text-center">
    <h1 class="result-header">Превышено количество попыток для прохождения теста</h1>
    <br>
    <hr>
  </div>
</template>

<script>
export default {
  name: "attempts_limit"
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Catamaran');

.container {
  font-family: 'Catamaran', sans-serif;
}

.result-header{
  margin-top: 2em;
}

</style>
