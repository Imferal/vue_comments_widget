<template>
  <div class="dialog" v-if="show">
    <div @click="modalHide" class="dialog__container">
      <div @click.stop class="dialog__body">
        <slot></slot>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "modal-dialog",
  props: {
    show: {
      type: Boolean,
      default: true,
    },
  },
  methods: {
    modalHide() {
      this.$emit("update:show", false);
    },
  },
};
</script>
<style lang="scss" scoped>
.dialog {
  &__container {
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    background: rgba(0, 40, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
    position: fixed;
  }

  &__body {
    background-color: white;
    max-width: 500px;
    padding: 1.5rem 1rem;
    border-radius: 0.4rem;
  }
}
</style>
