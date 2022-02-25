<template>
  <div class="modal-window" v-if="show" @click="hideModalWindow">
    <div class="modal-window-content" @click.stop>
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: "ModalWindow",
  props: {
    show: {
      type: Boolean,
      default: false,
    }
  },
  methods: {
    hideModalWindow() {
      this.$emit('update:show', false);
    }
  }
}
</script>

<style scoped>
.modal-window {
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  position: fixed;
  display: flex;
  background: rgba(0, 0, 0, 0.5);
}

.modal-window-content {
  margin: auto;
  background: white;
  border-radius: 0.3rem;
}
</style>