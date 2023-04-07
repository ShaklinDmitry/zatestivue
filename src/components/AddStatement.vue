<template>
  <div className="hello">
    <h1>{{ msg }}</h1>

    <input
        class="input"
        id="statement"
        type="text"
        v-model="statement"
    />
    <button v-on:click="saveStatement">Создать цитату</button>
    <SavedModal v-show="showModal" @close-modal="showModal = false"/>
  </div>
</template>

<script>
import axios from 'axios';
import SavedModal from '@/components/modals/SavedModal' ;


export default {
  name: 'ShowStatement',
  components: { SavedModal },
  props: {
    msg: String
  },
  data() {
    return {
      statement: '',
      showModal: false,
    };
  },
  methods: {
    saveStatement: function() {
      var data = JSON.stringify({"text":this.statement});
      var config = {
        method: 'post',
        url: process.env.VUE_APP_URL + '/api/statements',
        headers: {
          'Content-Type': 'application/json',
          Authorization: `Bearer ` + localStorage.getItem('apitoken')
        },
        data : data
      };

      axios(config)
          .then(() => {
            console.log('show modal')
            this.showModal = true
          })
          .catch(function (error) {
            console.log(error);
          });

    },
  }

}
</script>

