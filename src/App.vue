<template>
  <button class="btn" @click="showModal">Добавить</button>
  <ModalWindow v-model:show="modal">
    <FormPerson @add="addPerson"/>
  </ModalWindow>
  <PersonsList
      @remove="removePerson"
      v-if="persons.length > 0"
      :persons="this.persons"/>
  <h2 v-else>Список пока пуст</h2>
</template>

<script>
import {v4 as uuid} from 'uuid';
import PersonsList from "@/components/PersonsList";
import ModalWindow from "@/components/ModalWindow";
import FormPerson from "@/components/FormPerson";

export default {
  name: 'App',
  components: {PersonsList, ModalWindow, FormPerson},
  data() {
    return {
      modal:false,
      persons: [
        {
          id: uuid(),
          value: 'какое то название',
          address: 'какая то улица',
          ogrn: 'какой то огрн',
          inn: 'какой то инн',
          registrationDate: Date.now()
        },
        {
          id: uuid(),
          value: 'какое то название',
          address: 'какая то улица',
          ogrn: 'какой то огрн',
          inn: 'какой то инн',
          registrationDate: Date.now()
        },
        {
          id: uuid(),
          value: 'какое то название',
          address: 'какая то улица',
          ogrn: 'какой то огрн',
          inn: 'какой то инн',
          registrationDate: Date.now()
        },
      ]
    }
  },
  methods: {
    showModal(){
      console.log('showModal')
      this.modal = true;
    },
    addPerson(person){
      this.persons.push(person);
      this.modal = false;
    },
    removePerson(person){
      this.persons = this.persons.filter(p => p.id !== person.id);
    },
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,400;0,700;1,400;1,700&display=swap');
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  font-family: Roboto;
}
body{
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  width: 100vw;
}
#app {
  width: 50vw;
}
.btn{
  border: .25rem;
  padding: 0.65rem 1.25rem;
  cursor: pointer;
  border-radius: 0.25rem;
  background: rgba(58, 196, 249, 1);
  color: white;
  margin-bottom: 1rem;
  margin-top: 1rem;
}
.btn[type='submit']{
  margin-bottom: 0;
  margin-top: 0;
}
.btn:hover{
  background: rgba(46, 80, 248, 1);
}
.btn:active{
  background: rgba(0, 140, 255, 1);
}
</style>
