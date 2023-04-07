<template>
  <div className="hello">

    <button v-on:click="makeStatements">Разбить текст на высказывания</button>
    <SavedModal v-show="showModal" @close-modal="showModal = false"/>
  </div>
</template>


<script>
import axios from 'axios';
import SavedModal from '@/components/modals/SavedModal' ;


export default {
  name: 'makeStatements',
  components: { SavedModal },
  props: {
    msg: String
  },
  data() {
    return {
      showModal: false,
    };
  },
  methods: {
    makeStatements: function() {
      var config = {
        method: 'post',
        url: process.env.VUE_APP_URL+'/api/text/generate-statements',
        headers: {
          'Content-Type': 'application/json',
          Authorization: `Bearer ` + localStorage.getItem('apitoken')
        },
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

