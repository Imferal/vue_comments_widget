<template>
  <div class="post-list__container">
    <h2 v-if="posts.length === 0" class="post-list__title">
      Пока ни одного комментария - будьте первыми!
    </h2>
    <div v-else class="post-list">
      <h2 class="post-list__title">Комментарии</h2>
      <ul class="post-list__body" v-for="post in posts" :key="post.id">
        <post-item
          :post="post"
          @removePost="$emit('removePost', post)"
        ></post-item>
      </ul>
    </div>
  </div>
</template>

<script>
import PostItem from "./PostItem.vue";
export default {
  components: { PostItem },
  props: {
    posts: { type: Array, required: true },
  },
};
</script>

<style lang="scss" scoped>
.post-list {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}
</style>
