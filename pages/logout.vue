<template>
  <div class="container text-center">
    <h1 class="result-header">Выход из системы произведен успешно!</h1>
    <br>
    <hr>
  </div>
</template>

<script>
export default {
  name: "logout",
  methods: {
    async logout() {
      const response = await fetch('http://localhost:3000/api/v1/authentication/logout', {
        method: 'POST',
        credentials: 'include'
      })
    }
  },

  mounted() {
    this.logout()
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
