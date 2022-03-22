<template>
  <div class="person-item">

    <div class="person-item-row">
      <div><strong>Название: </strong></div>
      <div class="person-item-body">{{ person.value }}</div>
    </div>

    <div @click="editAddress" class="person-item-row address">

      <div v-if="isShowEditMode" class="address-wrapper-edit">
        <div class="form">
          <div>
            <strong>Адрес: </strong>
          </div>
          <input @input="setInputValue" :value="inputValue" type="text" placeholder="Введите адрес">
        </div>
        <div class="btn-wrapper">
          <button
              class="btn"
              @click.stop
              @click="saveAddress(person)"
          >Сохранить
          </button>
          <button
              class="btn"
              @click.stop
              @click="cancel"
          >Отмена
          </button>
        </div>
      </div>
      <div v-else-if="!isShowEditMode" class="address-wrapper">
        <div>
          <div class="address-wrapper"><strong>Адрес: </strong>
            <div class="person-item-body">{{ person.address }}</div>
          </div>
        </div>
        <button
            class="btn"
            @click.stop
            @click="editAddress"
            type="submit"
        >Редактировать
        </button>
      </div>

    </div>
    <div class="person-item-row">
      <div><strong>ОГРН: </strong></div>
      <div>{{ person.ogrn }}</div>
    </div>
    <div class="person-item-row">
      <div><strong>Инн: </strong></div>
      <div>{{ person.inn }}</div>
    </div>
    <div class="person-item-row">
      <div><strong>Дата регистрации: </strong></div>
      <div>{{ getRegistrationDate() }}</div>
    </div>
    <button class="btn" @click="$emit('remove',person)">Удалить</button>
  </div>
</template>

<script>
export default {
  name: "PersonItem",
  components: {},
  props: {
    person: {
      address: '',
      type: Object,
      required: true,
    }
  },
  data() {
    return {
      strBuffer: '',
      address: this.person.address,
      inputValue: this.person.address,
      isShowEditMode: false,
    }
  },

  methods: {
    getRegistrationDate() {
      const date = new Date(this.person.registrationDate);
      const day = date.getDate() < 10 ? `0${date.getDate()}` : `${date.getDate()}`;
      const month = date.getMonth() < 10 ? `0${date.getMonth() + 1}` : `${date.getMonth() + 1}`;
      const year = date.getFullYear();
      return `${day}-${month}-${year}`;

    },
    editAddress() {
      this.strBuffer = this.inputValue;
      this.isShowEditMode = true;
    },
    saveAddress(person) {
      person.address = this.inputValue;
      this.$emit('edit', this.person)
      this.isShowEditMode = false;
    },
    cancel() {
      this.inputValue = this.strBuffer;
      this.isShowEditMode = false;
    },
    setInputValue(e) {
      this.inputValue = e.target.value;
    }
  }
}
</script>

<style scoped>
.person-item {
  display: flex;
  flex-direction: column;
  justify-content: start;
  align-items: start;
}

.person-item-row {
  display: flex;
  flex-direction: row;
  justify-content: stretch;
  align-items: center;
  width: 100%;
  margin-bottom: 1rem;
}

.btn {
  border: .25rem;
  padding: 0.65rem 1.25rem;
  margin-bottom: 0;
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

strong {
  margin-right: 1rem;
}

.address-wrapper,
.address-wrapper-edit {
  width: 100%;
  height: 1rem;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
}
.btn-wrapper{
  display: flex;
  flex-direction: row;
  justify-content: end;
  align-items: center;
}
.btn-wrapper > button:first-child{
  margin-right: 1rem;
  background: red;
}

.form {
  display: flex;
  flex-direction: row;
  justify-content: stretch;
  align-items: center;
}

.form > input {
  height: 1rem;
  padding: 0.5rem;
}

.person-item-body {
  /*margin-left: .5rem;*/
}
</style>