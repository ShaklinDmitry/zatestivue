<template>
  <div>
    <b-table striped hover
             id="statements-table"
             :items="items"
             :fields="fields"
             :current-page="currentPage"
             :per-page="perPage"
             responsive>

      <template #cell(index)="data">
        {{ data.index + 1 }}
      </template>

      <template #cell(actions)="row">
        <b-button @click="editButton(row.item, row.index, $event.target)" variant="primary">Edit</b-button>
        <b-button @click="deleteButton(row.item, row.index, $event.target)" variant="danger">Delete</b-button>
      </template>

    </b-table>

    <b-modal :id="deleteModal.id" :title="deleteModal.title" @ok="handleOk" @hide="resetDeleteModal">
      <pre>You want to delete this element</pre>
    </b-modal>

    <b-modal :id="editModal.id" :title="editModal.title" @ok="handleEdit" @hide="resetEditModal">
      <pre>Edit modal</pre>
      <template>
        <div>
          <b-form-checkbox
              id="beststatement"
              v-model="beststatement_status"
              name="beststatement"
              value="yes"
              unchecked-value="nope"
          >
            best statement
          </b-form-checkbox>

          <div>State: <strong>{{ beststatement_status }}</strong></div>
        </div>
      </template>
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

import axios from "axios";
export default {
  data() {
    return {
      perPage: 14,
      currentPage: 1,
      fields: ['index', 'id', 'text', 'actions'],
      items:  [

      ],
      deleteModal: {
        id: 'delete-modal',
        title: '',
        item: ''
      },
      editModal:{
        id: 'edit-modal',
        title: '',
        item: ''
      },
      currentPickUpItem: '',
      beststatement_status: 'nope'
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
        url: process.env.VUE_APP_URL+'/api/statements',
        headers: { Authorization: `Bearer ` + localStorage.getItem('apitoken')},
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
    resetDeleteModal() {
      this.deleteModal.title = ''
      this.deleteModal.content = ''
    },
    resetEditModal() {
      this.editModal.title = ''
      this.editModal.content = ''
    },
    deleteButton(item, index, button) {
   //   console.log(this.currentPage * this.perPage + this.currentPickUpItem)
      this.deleteModal.title = `Row index: ${index}`
      this.deleteModal.item = item
      this.currentPickUpItem = index
      this.$root.$emit('bv::show::modal', this.deleteModal.id, button)
    },
    editButton(item, index, button){
      console.log(item)
      if(item.is_best_statement == 1)
        this.beststatement_status = 'yes'
      else
        this.beststatement_status = 'nope'

      this.editModal.title = `Row index: ${index}`
      this.editModal.item = item
      this.currentPickUpItem = index
      this.$root.$emit('bv::show::modal', this.editModal.id, button)
    },
    handleOk() {

      //здесь вызвать метод удаления высказывания
      var data = '';

      var config = {
        method: 'delete',
        url: process.env.VUE_APP_URL+'/api/statements/' + this.deleteModal.item.id,
        headers: { Authorization: `Bearer ` + localStorage.getItem('apitoken')},
        data : data
      };

      axios(config)
          .then(() => {
                // console.log('item deleted');
                // console.log('('+this.currentPage +' - 1) * ' + this.perPage + '+' + this.currentPickUpItem)
            this.items.splice((this.currentPage - 1) * this.perPage + this.currentPickUpItem, 1);
          })
          .catch(function (error) {
            console.log(error);
          });

    },
    handleEdit(){
      console.log('edit button')

      var data = '';

      let urlForChangeStatementType = 0;
      if(this.editModal.item.is_best_statement){
        urlForChangeStatementType = process.env.VUE_APP_URL+'/api/beststatements/' + this.editModal.item.id + '/make-normalstatement'
      }else{
        urlForChangeStatementType = process.env.VUE_APP_URL+'/api/statement/' + this.editModal.item.id + '/make-beststatement'
      }

      console.log(urlForChangeStatementType)

      var config = {
        method: 'patch',
        url: urlForChangeStatementType,
        headers: { Authorization: `Bearer ` + localStorage.getItem('apitoken')},
        data : data
      };

      axios(config)
          .then(() => {
             console.log('item switched type');
            if(this.editModal.item.is_best_statement){
              this.editModal.item.is_best_statement = 0
            }else{
              this.editModal.item.is_best_statement = 1
            }

          })
          .catch(function (error) {
            console.log(error);
          });


    }
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
