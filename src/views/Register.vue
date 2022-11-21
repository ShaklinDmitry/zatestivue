<template>
  <div class="register">
    <form @submit.prevent="register" v-if="!formSubmitted">
      <h3>Sign Up</h3>

      <div class="form-group">
        <label>Full Name</label>
        <input type="text" v-model="userName" class="form-control form-control-lg"/>
      </div>

      <div class="form-group">
        <label>Email address</label>
        <input type="email" v-model="email" class="form-control form-control-lg" />
      </div>

      <div class="form-group">
        <label>Password</label>
        <input type="password" v-model="password" class="form-control form-control-lg" />
      </div>

      <button type="submit" class="btn btn-dark btn-lg btn-block" id="sign-up">Sign Up</button>

      <p class="forgot-password text-right">
        Already registered
<!--        <router-link :to="{name: 'login'}">sign in?</router-link>-->
      </p>
    </form>
    <div v-if="formSubmitted">
      <h3>You registred</h3>
      <p>Name: {{ userName }}</p>
      <p>Email: {{ email }}</p>
      <small>Click on run to launch the app again.</small>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src

import axios from "axios";

export default {
  name: 'Register',
  components: {

  },
  data() {
    return {
      userName: '',
      email: '',
      password: '',
      formSubmitted: false
    };
  },
  methods: {
    register: function() {
      var data = JSON.stringify({"name":this.userName,
                                        "email": this.email,
                                        "password": this.password});
      var config = {
        method: 'post',
        url: 'http://89.108.99.73:8100/api/register',
        headers: {
          'Content-Type': 'application/json'
        },
        data : data
      };

      axios(config)
          .then(() => {
            this.formSubmitted = true
          })
          .catch(function (error) {
            console.log(error);
          });

    },
  }
}
</script>
