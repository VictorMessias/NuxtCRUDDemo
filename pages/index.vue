<template>
 <div>
<b-navbar toggleable="lg" type="dark" variant="info">
    <b-navbar-brand href="#">NavBar</b-navbar-brand>

</b-navbar>


<b-container class="Main-container">

<b-button type="button" v-b-modal.modal variant="success">Novo</b-button>

<b-row class="Main-container-header">
  <b-col cols="3">
  Imagem
  </b-col>
  <b-col cols="2">
    Primeiro nome
  </b-col>

  <b-col cols="2">
    Último nome
  </b-col>

  <b-col cols="3">
    Email
  </b-col>
  <b-col cols="2">
    Ações
  </b-col>
</b-row>

<b-row class="Main-container-body" v-for="item in users" :key="item.id">
  <b-col cols="3">
  <img :src="item.avatar" class="fluid" />
  </b-col>
  <b-col cols="2">
    {{ item.first_name }}
  </b-col>

  <b-col cols="2">
    {{ item.last_name }}
  </b-col>

  <b-col cols="3">
    {{ item.email }}
  </b-col>

  <b-col cols="2">
    <b-button type="button" v-b-modal.modal @click="editModal(item.id)" variant="info">Editar</b-button>
    <b-button type="button" @click="removeUser(item)" variant="danger">Excluir</b-button>
  </b-col>
</b-row>


</b-container>

<modal :user_id="user_id" @reload="fetchUsers"/>

 </div>

</template>


<script>
import Modal from '@/components/modal';
import axios from 'axios';

export default {
  components:{
    Modal
  },
  data(){
    return{
      url: 'https://reqres.in',
      users: [],
      user_id: -1
    }
  },
  methods:{
    editModal(id){
      this.$bvModal.show('modal');
      this.user_id = id;
    },
    removeUser(item){

      if(confirm('Deseja realmente remover este usuário?')){

         axios.delete(this.url + '/api/users/' + item.id).then(response => {
          this.$delete(this.users, this.users.indexOf(item));

        }).catch(err => {
          console.log(err);
        });

      }


    },
    fetchUsers(){

      axios.get(this.url + '/api/users?page=1').then(response => {
        this.users = response.data.data;

      }).catch(err => {
        console.log(err);
      });


    }
  },
  mounted(){
    this.fetchUsers();
  }

}
</script>

<style>

body{
  background: #ddd;
}


.Main-container{
  padding: 20px;
  background: #fff;
  border-radius: 5px;
  border: 1px #ccc solid;
  margin-top: 10px;
}
.Main-container-header{
  border: 1px #ddd solid;
  padding: 10px;
}

.Main-container-body{
  border-bottom: 1px #ddd solid;
  margin-bottom: 10px;
  padding: 10px 0;
}



</style>