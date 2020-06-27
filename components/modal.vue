<template>
    <b-modal id="modal" title="Criar/Editar">

        <b-form>
        <b-form-group label="Nome" label-for="name">
            <b-form-input id="name" v-model="name"></b-form-input>
        </b-form-group>

         <b-form-group label="Ocupação" label-for="job">
            <b-form-input id="job" v-model="job"></b-form-input>
        </b-form-group>
        </b-form>

          <template v-slot:modal-footer>
              <b-button type="button" @click="closeModal" variant="info">Cancelar</b-button>
              <b-button type="button" @click="onSubmit" variant="success">Salvar</b-button>
        </template>


  </b-modal>
</template>


<script>
import axios from 'axios';

export default {
    props:{
        user_id:{
            type: Number,
            default: -1
        }
    },
    data(){
        return{
            name: '',
            job: '',
            url: 'https://reqres.in/api/users',
            update: false
        }
    },
    watch:{
        user_id(){
            this.update = true;
            this.loadUser();
            
        }
    },
    methods:{
        closeModal(){
            this.$bvModal.hide('modal');
        },
        loadUser(){

            axios.get(this.url + '/' + this.user_id).then(response => {
                this.name = response.data.data.first_name;
                this.job = response.data.ad.company;
                
            }).catch(err => {
                console.log(err);
            });


        },
        onSubmit(){

            if(this.update){

                let data = {
                    name: this.name,
                    job: this.job
                };

                axios.put(this.url + '/' + this.user_id, data).then(response => {
                    this.name = '';
                    this.job = '';
                    this.$bvModal.hide('modal');
                    this.$emit('reload');
                    this.update = false;
                    
                }).catch(err => {
                    console.log(err);
                });


            }else{

                let data = {
                    name: this.name,
                    job: this.job
                };

                axios.post(this.url, data).then(response => {
                    this.name = '';
                    this.job = '';
                    this.$bvModal.hide('modal');
                    this.$emit('reload');
                    
                }).catch(err => {
                    console.log(err);
                });

            }

            



        }
    }
}
</script>