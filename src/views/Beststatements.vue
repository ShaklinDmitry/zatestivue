<template> 
  <div class="beststatements">
    <b-table striped hover
             id="beststatements-table"
             :items="items"
             :fields="fields"
             :current-page="currentPage"
             :per-page="perPage"
             responsive>
      <template #cell(actions)="row">
        <b-button variant="primary">Edit</b-button>
        <b-button @click="info(row.item, row.index, $event.target)" variant="danger">Delete</b-button>
      </template>
    </b-table>
    <b-modal :id="infoModal.id" :title="infoModal.title" @ok="handleOk" @hide="resetInfoModal">
      <pre>You want to delete this element</pre>
    </b-modal>

    <b-pagination
        v-model="currentPage"
        :total-rows="rows"
        :per-page="perPage"
        aria-controls="statements-table"
    ></b-pagination>
  </div>
</template>

<script>
// @ is an alias to /src


import axios from "axios";

export default {
  name: 'Beststatements',
  components: {

  },
  data() {
    return {
      perPage: 4,
      currentPage: 1,
      fields: ['id', 'text', 'actions'],
      items:  [],
      infoModal: {
        id: 'info-modal',
        title: '',
        item: ''
      },
      currentPickUpItem: ''
    }
  },
  beforeMount(){
    this.getBestStatements()
  },
  computed: {
    rows() {
      return this.items.length
    }
  },
  methods: {
    getBestStatements: function() {
      var data = '';

      var config = {
        method: 'get',
        url: process.env.VUE_APP_URL+'/api/beststatements',
        headers: { Authorization: `Bearer ` + localStorage.getItem('apitoken')},
        data : data
      };

      axios(config)
          .then((response) => {
              this.items = response.data.data.bestStatements;
          })
          .catch(function (error) {
            console.log(error);
          });

    },
    resetInfoModal() {
      this.infoModal.title = ''
      this.infoModal.content = ''
    },
    info(item, index, button) {
      this.infoModal.title = `Row index: ${index}`
      this.infoModal.item = item
      this.currentPickUpItem = index
      this.$root.$emit('bv::show::modal', this.infoModal.id, button)
    },
    handleOk() {
      // console.log(this.items)
      // console.log(this.infoModal.item.id)

      //здесь вызвать метод удаления высказывания
      var data = '';

      var config = {
        method: 'delete',
        url: process.env.VUE_APP_URL+'/api/beststatements/' + this.infoModal.item.id,
        headers: { Authorization: `Bearer ` + localStorage.getItem('apitoken')},
        data : data
      };

      axios(config)
          .then(() => {

        //    console.log('item deleted');
            this.items.splice((this.currentPage - 1) * this.perPage + this.currentPickUpItem, 1);
          })
          .catch(function (error) {
            console.log(error);
          });

    },
  }
}
</script>
