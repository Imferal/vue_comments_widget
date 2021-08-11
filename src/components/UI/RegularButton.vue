<template>
  <button>
    <slot></slot>
  </button>
</template>

<script>
export default {
  name: "regular-button",
};
</script>

<style lang="scss" scoped>
button {
  color: white;
  background-color: rgb(3, 155, 3);
  transition: 0.4s;
  line-height: 2rem;
  border: none;
  padding: 0 2rem;
  &:hover {
    background-color: green;
  }
}
</style>
