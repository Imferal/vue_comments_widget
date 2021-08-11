<template>
  <button>
    <slot></slot>
  </button>
</template>

<script>
export default {
  name: "small-button",
};
</script>

<style lang="scss" scoped>
button {
  color: white;
  font-size: 0.7rem;
  background-color: rgb(3, 155, 3);
  transition: 0.4s;
  line-height: 2em;
  border: none;
  padding: 0 2em;
  &:hover {
    background-color: green;
  }
}
</style>
