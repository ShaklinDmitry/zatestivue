<template>
  <div>
    <b-table striped hover
             id="statements-table"
             :items="items"
             :fields="fields"
             :current-page="currentPage"
             :per-page="perPage"
             responsive></b-table>
    <b-pagination
        v-model="currentPage"
        :total-rows="rows"
        :per-page="perPage"
        aria-controls="statements-table"
    ></b-pagination>
  </div>
</template>

<script>

import axios from "axios";
export default {
  data() {
    return {
      perPage: 4,
      currentPage: 1,
      fields: ['id', 'text', 'edit'],
      url: process.env.VUE_APP_URL,
      items:  [

      ]
    }
  },
  beforeMount(){
    this.getStatements()
  },
  computed: {
    rows() {
      return this.items.length
    }
  },
  methods: {
    getStatements: function() {
      var data = '';

      var config = {
        method: 'get',
        url: this.url+'/api/statements?userId=3',
        headers: {},
        data : data
      };

      axios(config)
          .then((response) => {
            this.items = response.data.data.statements;
          })
          .catch(function (error) {
            console.log(error);
          });




    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

table {
  font-family: 'Open Sans', sans-serif;
  width: 750px;
  border-collapse: collapse;
  border: 3px solid #44475C;
  margin: 10px 10px 0 10px;
}

table th {
  text-transform: uppercase;
  text-align: left;
  background: #44475C;
  color: #FFF;
  padding: 8px;
  min-width: 30px;
}

table td {
  text-align: left;
  padding: 8px;
  border-right: 2px solid #7D82A8;
}
table td:last-child {
  border-right: none;
}
table tbody tr:nth-child(2n) td {
  background: #D4D8F9;
}
</style>
