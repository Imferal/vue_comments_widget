<template>
  <div id="app" class="app">
    <h1>Комментарии</h1>
    <input
      type="text"
      placeholder="Поиск по заголовку..."
      v-model="searchQuery"
    />
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
    <!-- <div class="pagination__wrapper">
      <div
        v-for="page in totalPages"
        :key="page"
        class="pagination__page"
        :class="{ pagination__page_current: page === pageNumber }"
        @click="changePage(page)"
      >
        {{ page }}
      </div>
    </div> -->
    <post-list
      v-if="!isPostsFetching"
      :posts="sortedAndSearchedPosts"
      @removePost="removePost"
    />
    <p v-else>Загружаем комментари....</p>
    <div ref="observer" class="observer"></div>
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
      searchQuery: "",
      selectedSort: "",
      pageNumber: 1,
      pageLimit: 10,
      totalPages: null,
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
    // changePage(pageNumber) {
    //   this.pageNumber = pageNumber;
    // },
    async fetchPosts() {
      try {
        this.isPostsFetching = true;
        const response = await axios.get(
          `https://jsonplaceholder.typicode.com/posts`,
          {
            params: {
              _page: this.pageNumber,
              _limit: this.pageLimit,
            },
          }
        );
        this.totalPages = Math.ceil(
          response.headers["x-total-count"] / this.pageLimit
        );
        this.posts = response.data;
      } catch (error) {
        alert("Ошибка");
      } finally {
        this.isPostsFetching = false;
      }
    },
    async fetchMorePosts() {
      try {
        this.page += 1;
        const response = await axios.get(
          `https://jsonplaceholder.typicode.com/posts`,
          {
            params: {
              _page: this.pageNumber,
              _limit: this.pageLimit,
            },
          }
        );
        this.totalPages = Math.ceil(
          response.headers["x-total-count"] / this.pageLimit
        );
        this.posts = [...this.posts, ...response.data];
      } catch (error) {
        alert("Ошибка");
      }
    },
  },
  mounted() {
    this.fetchPosts();
    const options = {
      rootMargin: "0px",
      threshold: 1.0,
    };
    const callback = (entries) => {
      if (entries[0].isIntersecting && this.page < this.totalPages) {
        this.fetchMorePosts();
      }
    };
    const observer = new IntersectionObserver(callback, options);
    observer.observe(this.$refs.observer);
  },
  computed: {
    // Подготовка постов к выводу
    // Сортируем
    sortedPosts() {
      return [...this.posts].sort((post1, post2) => {
        return post1[this.selectedSort]?.localeCompare(
          post2[this.selectedSort]
        );
      });
    },
    // Фильтруем по названию, если поле поиска не пустое
    sortedAndSearchedPosts() {
      return this.sortedPosts.filter((post) =>
        post.title.toLowerCase().includes(this.searchQuery.toLowerCase())
      );
    },
  },
  watch: {
    // // Отслеживаем смену страницы в пагинаторе
    // pageNumber() {
    //   this.fetchPosts();
    // },
  },
  // Вариант сортировки с мутацией массива
  // watch: {
  //   selectedSort(newValue) {
  //     // Сортировка постов по выбранному признаку
  //     this.posts.sort((post1, post2) => {
  //       // Сравниваем название или описание одного поста с другим
  //       return post1[newValue]?.localeCompare(post2[newValue]);
  //     });
  //   },
  // },
};
</script>

<style lang="scss">
* {
  padding: 0;
  margin: 0;
  font-family: sans-serif;
  transition: 0.2s;
}

.app {
  padding: 1rem;
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

input,
textarea {
  border: 1px solid green;
  padding: 0.4rem 0.6rem;
  resize: vertical;
  &:hover,
  &:focus {
    box-shadow: 0 0 14px rgba(green, 0.2);
  }
}

.pagination {
  &__wrapper {
    display: flex;
    gap: 0.3rem;
    justify-content: flex-end;
  }

  &__page {
    padding: 0.4rem;
    border: 1px solid green;

    &_current {
      background-color: green;
      color: white;
    }
  }
}

.observer {
  height: 30px;
  background-color: green;
}
</style>
