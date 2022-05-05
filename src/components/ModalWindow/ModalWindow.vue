<template>
  <div
      class="modal-wrapper"
      @click="onClickCloseModal"
      v-if="isModalVisible"
  >
    <div class="modal-content" @click.stop>
    <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: "ModalWindow",

  props: {
    isModalVisible: {
      type: Boolean,
      default: false
    }
  },
  methods: {
    onClickCloseModal() {
      this.$emit('onClickToggleModal')
    },
  }
}
</script>

<style scoped>
.modal-wrapper {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  display: flex;
  background-color: rgba(0, 0, 0, 0.5);
  overflow-y: auto;
  z-index: 100;
}
.modal-content {
  position: relative;
  margin: auto;
  border-radius: 5px;
}
</style>