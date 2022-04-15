<template>
  <div className="hello">
    <h1>{{ msg }}</h1>

    <input
        class="input"
        id="statement"
        type="text"
        v-model="statement"
    />
    <button v-on:click="sendStatement">Создать цитату</button>
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
    sendStatement: function() {
      console.log('send statement')
      var data = JSON.stringify({"text":this.statement});
      var config = {
        method: 'post',
        url: 'http://89.108.114.164:8000/api/statements',
        headers: {
          'Content-Type': 'application/json'
        },
        data : data
      };

      axios(config)
          .then((response) => {
            this.showModal = true
            console.log(JSON.stringify(response.data));
          })
          .catch(function (error) {
            console.log(error);
          });

    },
  }

}
</script>

