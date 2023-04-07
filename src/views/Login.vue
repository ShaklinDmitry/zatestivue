<template>
  <div class="register">
    <form @submit.prevent="login" v-if="!formSubmitted">
      <h3>Login</h3>

      <div class="form-group">
        <label>Email address</label>
        <input type="email" v-model="email" class="form-control form-control-lg" />
      </div>

      <div class="form-group">
        <label>Password</label>
        <input type="password" v-model="password" class="form-control form-control-lg" />
      </div>

      <button type="submit" class="btn btn-dark btn-lg btn-block" id="sign-up">Login</button>

    </form>
    <div v-if="formSubmitted">
      <h3>You logged</h3>
      <p>Email: {{ email }}</p>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src

import axios from "axios";

export default {
  name: 'Login',
  components: {

  },
  data() {
    return {
      email: '',
      password: '',
      formSubmitted: false
    };
  },
  methods: {
    login: function() {
      var data = JSON.stringify(
          {"email": this.email,
                 "password": this.password
                 });
      var config = {
        method: 'post',
        url: 'http://89.108.99.73:8100/api/login',
        headers: {
          'Content-Type': 'application/json'
        },
        data : data
      };

      axios(config)
          .then((success) => {
            this.formSubmitted = true;
            console.log(success.data);
            localStorage.setItem('apitoken', success.data.access_token)
          })
          .catch(function (error) {
            console.log(error.response.data);
          });

    },
  }
}
</script>
