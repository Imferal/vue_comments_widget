<template>
  <div id="app" class="app">
    <h1>Комментарии</h1>
    <div class="app__buttons">
      <regular-button @click="showModal">Создать пост</regular-button>
      <posts-select
        :selected-sort="selectedSort"
        @update-value="updateValue"
        :options="sortOptions"
      />
    </div>
    <modal-dialog v-model:show="modalVisible">
      <add-post @createPost="createPost" />
    </modal-dialog>
    <post-list
      v-if="!isPostsFetching"
      :posts="posts"
      @removePost="removePost"
    />
    <p v-else>Загружаем комментари....</p>
  </div>
</template>

<script>
import AddPost from "@/components/AddPost";
import PostList from "@/components/PostList";
import RegularButton from "@/components/UI/RegularButton";
import axios from "axios";
import ModalDialog from "./components/UI/ModalDialog.vue";
import PostsSelect from "./components/UI/PostsSelect.vue";

export default {
  components: { AddPost, PostList, RegularButton, ModalDialog, PostsSelect },
  data() {
    return {
      posts: [],
      modalVisible: false,
      isPostsFetching: false,
      selectedSort: "",
      sortOptions: [
        { value: "title", name: "По названию" },
        { value: "body", name: "По содержимому" },
      ],
    };
  },
  methods: {
    createPost(newPost) {
      this.posts.push(newPost);
      this.modalVisible = false;
    },
    removePost(post) {
      this.posts = this.posts.filter((p) => p.id !== post.id);
    },
    showModal() {
      this.modalVisible = true;
    },
    updateValue(value) {
      this.selectedSort = value;
    },
    async fetchPosts() {
      try {
        this.isPostsFetching = true;
        const response = await axios.get(
          "https://jsonplaceholder.typicode.com/posts?_limit=3"
        );
        this.posts = response.data;
      } catch (error) {
        alert("Ошибка");
      } finally {
        this.isPostsFetching = false;
      }
    },
  },
  mounted() {
    this.fetchPosts();
  },
  watch: {
    selectedSort(newValue) {
      // Сортировка постов по выбранному признаку
      this.posts.sort((post1, post2) => {
        // Сравниваем название или описание одного поста с другим
        return post1[newValue]?.localeCompare(post2[newValue]);
      });
    },
  },
};
</script>

<style lang="scss">
* {
  padding: 0;
  margin: 0;
  font-family: sans-serif;
}

.app {
  padding: 1rem 1rem 0;
  display: flex;
  flex-direction: column;
  gap: 2rem;
  max-width: 800px;
  margin: 0 auto;

  &__buttons {
    display: flex;
    justify-content: space-between;
  }
}

textarea {
  resize: vertical;
}
</style>
