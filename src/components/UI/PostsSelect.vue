<template>
  <select v-bind:value="selectedSort" @change="changeOption">
    <option value="" disabled>Выберите что-нибудь</option>
    <option v-for="option in options" :value="option.value" :key="option.value">
      {{ option.name }}
    </option>
  </select>
</template>
<script>
export default {
  name: "posts-select",
  props: {
    selectedSort: {
      type: String,
      default: "",
    },
    options: {
      type: Array,
      default: () => [],
    },
  },
  methods: {
    changeOption(event) {
      this.$emit("update-value", event.target.value);
    },
  },
};
</script>
<style lang="scss" scoped></style>
