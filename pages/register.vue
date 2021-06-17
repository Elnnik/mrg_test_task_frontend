<template>
  <div class="container mt-4 d-flex justify-content-center">
    <div>
      <div class="row register-header container justify-content-center">
        <h3>Регистрация</h3>
      </div>

      <div class="col-sm-9 mx-auto register-form">
        <form @submit.prevent="submit" class="text-center container">
          <div class="mb-3">
            <label for="inputFirstName" class="form-label">Имя</label>
            <input type="text" v-model="firstName" class="form-control" id="inputFirstName">
          </div>

          <div class="mb-3">
            <label for="inputLastName" class="form-label">Фамилия</label>
            <input type="text" v-model="lastName" class="form-control" id="inputLastName">
          </div>

          <div class="mb-3">
            <label for="inputLogin" class="form-label">Логин</label>
            <input type="text" v-model="login" class="form-control" id="inputLogin">
          </div>

          <div class="mb-3">
            <label for="inputEmail" class="form-label">Email</label>
            <input type="text" v-model="email" class="form-control" id="inputEmail">
          </div>

          <div class="mb-3">
            <label for="inputPassword" class="form-label">Пароль</label>
            <input type="password" v-model="password" class="form-control" id="inputPassword">
          </div>

          <div class="mb-3">
            <label for="inputPasswordConfirmation" class="form-label">Подтверждение пароля</label>
            <input type="password" v-model="passwordConfirmation" class="form-control" id="inputPasswordConfirmation">
          </div>

          <button type="submit" class="btn btn-primary register-button">Зарегистрироваться</button>
        </form>
      </div>
    </div>
    </div>
</template>

<script>
export default {
  name: "register",
  layout: 'default',
  data() {
    return {
      firstName: '',
      lastName: '',
      login: '',
      email: '',
      password: '',
      passwordConfirmation: ''
    }
  },

  methods: {
    async submit() {
      console.log(this.firstName, this.lastName)
      const response = await fetch('http://localhost:3000/api/v1/authentication/signup', {
        method: 'POST',
        headers: {'Content-Type': 'application/json'},
        credentials: 'include',
        body: JSON.stringify({
          firstName: this.firstName,
          lastName: this.lastName,
          login: this.login,
          email: this.email,
          password: this.password,
          passwordConfirmation: this.passwordConfirmation
        })
      });

      if (response.status === 200) {
        await this.$router.push('/register_success')
      } else {
        await this.$router.push('/register_failed')
      }
    }
  }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Catamaran');

.register-header {
  margin-bottom: 2em;
}

.register-form {
  padding: 2em;
  margin-bottom: 2em;
  border: 1px solid #a8a8a8;
  border-radius: .5em;
  max-width: 700px;
  box-sizing: border-box;
  font-family: 'Catamaran', serif;
}

.register-button {
  margin-top: 1em;
}

</style>
