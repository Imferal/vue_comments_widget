<template>
  <form class="add-post" @submit.prevent>
    <h2 class="add-post__title">Добавить новый пост</h2>
    <input
      type="text"
      class="add-post__input"
      placeholder="Заголовок поста"
      v-model="post.title"
    />
    <textarea
      class="add-post__body"
      placeholder="Текст поста"
      v-model="post.body"
    ></textarea>
    <regular-button @click="createPost">Добавить пост</regular-button>
  </form>
</template>

<script>
import RegularButton from "./UI/RegularButton.vue";

export default {
  components: {
    RegularButton,
  },
  data() {
    return {
      post: {
        title: "",
        body: "",
        id: null,
      },
    };
  },
  methods: {
    createPost() {
      this.post.id = Date.now();
      this.$emit("createPost", this.post);
    },
  },
  // watch: {
  //   post: {
  //     handler(newVal) {
  //       console.log(newVal);
  //     },
  //     deep: true,
  //   },
  // },
};
</script>

<style lang="scss">
.add-post {
  display: flex;
  flex-direction: column;
  align-content: stretch;
  gap: 1rem;
}

.add-post__input {
  border: 1px solid green;
  padding: 0.4rem 0.6rem;
}

.add-post__body {
  border: 1px solid green;
  padding: 0.4rem 0.6rem;
}

input:focus,
textarea:focus,
input:hover,
textarea:hover {
  &:hover {
    box-shadow: 0 0 14px rgba(green, 0.2);
  }
}
</style>
