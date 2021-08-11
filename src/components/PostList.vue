<template>
  <section class="post-list__container">
    <h2 v-if="posts.length === 0" class="post-list__title">
      Пока ни одного комментария - будьте первыми!
    </h2>
    <div v-else class="post-list">
      <h2 class="post-list__title">Комментарии</h2>
      <transition-group
        name="post-list__animation"
        tag="ul"
        class="post-list__body"
      >
        <post-item
          v-for="post in posts"
          :key="post.id"
          :post="post"
          @removePost="$emit('removePost', post)"
        ></post-item>
      </transition-group>
    </div>
  </section>
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
.post-list,
.post-list__body {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

// Анимация удаления элемента
.post-list__animation-item {
  display: inline-block;
  margin-right: 10px;
}
.post-list__animation-enter-active,
.post-list__animation-leave-active {
  transition: all 0.4s;
}

.post-list__animation-enter,
.post-list__animation-leave-to {
  opacity: 0;
  transform: translateX(130px);
}

// Анимация перемещения элементов
.post-list__animation-move {
  transition: transform 0.8s ease;
}
</style>
