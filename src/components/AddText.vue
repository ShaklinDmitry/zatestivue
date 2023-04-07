<template>
  <div className="hello">

    <textarea
        class="input"
        id="text"
        type="text"
        v-model="text"
    />
    <button v-on:click="saveText">Добавить текст</button>
    <SavedModal v-show="showModal" @close-modal="showModal = false"/>
  </div>
</template>


<script>
import axios from 'axios';
import SavedModal from '@/components/modals/SavedModal' ;


export default {
  name: 'ShowText',
  components: { SavedModal },
  props: {
    msg: String
  },
  data() {
    return {
      text: '',
      showModal: false,
    };
  },
  methods: {
    saveText: function() {
      var data = JSON.stringify({"text":this.text});
      var config = {
        method: 'post',
        url: process.env.VUE_APP_URL+'/api/statements/text',
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

