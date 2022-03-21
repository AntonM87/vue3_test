<template>
  <div class="form">
    <h2>Добавить юр. лицо</h2>
    <div class="form-wrapper">
      <input class="form-wrapper-input" v-model="person.value" type="text" placeholder="Название">
      <input class="form-wrapper-input" v-model="person.address" type="text" placeholder="Адрес">
      <input class="form-wrapper-input" v-model="person.ogrn" type="text" placeholder="ОГРН">
      <div class="form-wrapper-input-container">
        <input class="form-wrapper-input" v-model="person.inn" type="text" placeholder="Инн">
        <button class="btn" @click="fetchPerson">Загрузить</button>
      </div>
    </div>
    <button class="btn add" @click="addPerson" type="submit">Добавить</button>
  </div>
</template>

<script>
import token from "@/token/token";
export default {
  name: "FormPerson",
  data() {
    return {
      person: {
        id: Date.now(),
        value: '',
        address: '',
        ogrn: '',
        inn: '',
        registrationDate: new Date(),
      },
      requestPerson: ''
    }
  },
  methods: {
    addPerson() {
      if (
          this.person.value === '' ||
          this.person.address === '' ||
          this.person.ogrn === ''
      ) {
        alert('Заполните все поля');
        return;
      }
      this.$emit('add', this.person)
      this.person = {
        id: '',
        value: '',
        address: '',
        ogrn: '',
        inn: '',
        registrationDate: '',
      }
    },
    getRegistrationDate() {
      const date = new Date();
      const day = date.getDate() < 10 ? `0${date.getDate()}` : `${date.getDate()}`;
      const month = date.getDay() < 10 ? `0${date.getMonth() + 1}` : `${date.getMonth() + 1}`;
      const year = date.getFullYear();
      return `${day}-${month}-${year}`;
    },
    parseResponse(response) {
      this.person.value = response.suggestions[0].value;
      const country = response.suggestions[0].data.address.data.country;
      const address = response.suggestions[0].data.address.data.source;
      this.person.address = `${country} ${address}`;
      this.person.ogrn = response.suggestions[0].data.ogrn;
      this.person.inn = response.suggestions[0].data.inn;
      this.person.registrationDate = response.suggestions[0].data.ogrn_date;

    },

    async fetchPerson() {

      if (this.person.inn === ''){
        alert('ИНН необходимо заполнить');
        return false
      }

      const url = "https://suggestions.dadata.ru/suggestions/api/4_1/rs/findById/party";
      const tokenQuery = token.token;
      const query = this.person.inn;

      const options = {
        method: "POST",
        mode: "cors",
        headers: {
          "Content-Type": "application/json",
          "Accept": "application/json",
          "Authorization": "Token " + tokenQuery
        },
        body: JSON.stringify({query})
      }

      await fetch(url, options)
          .then(response => response.text())
          .then(result => this.requestPerson = JSON.parse(result))
          .catch(error => console.log("error", error));

      this.parseResponse(this.requestPerson);
    }
  }
}
</script>

<style scoped>
.form {
  padding: 1rem;
}

.form h2 {
  text-align: center;
}

.form-wrapper {
  margin: 1rem;
  display: flex;
  flex-direction: column;
  justify-content: stretch;
  align-items: stretch;
}

.form-wrapper-input {
  height: 2rem;
  margin-bottom: 1rem;
  padding: 1rem;
}

.form-wrapper-input-container > .form-wrapper-input {
  width: 60%;
  margin-right: 1rem;
}

.btn {
  border: .25rem;
  padding: 0.65rem 1.25rem;
  cursor: pointer;
  border-radius: 0.25rem;
  background: rgba(58, 196, 249, 1);
  color: white;
}

.btn:hover {
  background: rgba(46, 80, 248, 1);
}

.btn:active {
  background: rgba(0, 140, 255, 1);
}

.add {
  text-align: right;
  margin-left: 0.75rem;
  margin-bottom: 1rem;
}
</style>