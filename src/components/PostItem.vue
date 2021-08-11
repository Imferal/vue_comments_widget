<template>
  <li class="post__wrapper">
    <article class="post">
      <h4 class="post__title">{{ post.title }}</h4>
      <p class="post__body">{{ post.body }}</p>
      <div class="post__buttons">
        <small-button @click="$emit('removePost', post)"
          >Удалить пост</small-button
        >
      </div>
    </article>
  </li>
</template>
<script>
export default {
  props: {
    post: {
      type: Object,
      required: true,
    },
  },
};
</script>
<style lang="scss">
.post {
  border: 1px solid green;
  padding: 0.5rem 1rem 1rem;
  display: flex;
  flex-direction: column;
  gap: 1rem;
  transition: 0.5s;

  &:hover {
    box-shadow: 0 0 14px rgba(green, 0.2);
  }

  &__wrapper {
    list-style: none;
  }

  &__title {
    line-height: 2em;
    border-bottom: 1px solid rgba(green, 0.6);
  }

  &__body {
    text-align: end;
  }
}
</style>
