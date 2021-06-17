<template>
  <div class="container mt-4 d-flex justify-content-center">
    <div>
      <div class="row authorization-header container justify-content-center">
        <h3>Вход в систему</h3>
      </div>
      <br>
      <div class="col-sm-12 mx-auto authorization-form">
        <form @submit.prevent="submit" class="container text-center">
          <div class="mb-3">
            <label for="inputEmail" class="form-label">Email</label>
            <input type="text" v-model="loginEmail" class="form-control" id="inputEmail">
          </div>

          <div class="mb-3">
            <label for="inputPassword" class="form-label">Пароль</label>
            <input type="password" v-model="loginPassword" class="form-control" id="inputPassword">
          </div>
          <button type="submit" class="btn btn-primary login-button">Войти</button>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import VueCookies from 'vue-cookies'

Vue.use(VueCookies);

export default {
  name: "login",
  layout: 'default',
  data(){
    return {
      loginEmail: '',
      loginPassword: ''
    }
  },

  methods: {
    async submit() {
      const response = await fetch('http://217.160.214.250:3000/api/v1/authentication/login', {
        method: 'POST',
        headers: {'Content-Type': 'application/json'},
        credentials: 'include',
        body: JSON.stringify({
          email: this.loginEmail,
          password: this.loginPassword
        })
      });

      if (response.status === 200) {
        await this.$router.push('/login_success')
      } else {
        await this.$router.push('/login_failed')
      }


    }
  }
}
</script>

<style scoped>

.authorization-header {
  margin-bottom: 1em;
}

.authorization-form {
  padding: 2em;
  margin-bottom: 2em;
  border: 1px solid #a8a8a8;
  border-radius: .5em;
  max-width: 700px;
  box-sizing: border-box;
  font-family: 'Catamaran', serif;
}

.login-button {
  margin-top: 1em;
}

</style>
