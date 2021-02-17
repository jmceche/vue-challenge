<template>
  <div id="app">
    <Navbar />
    <div class="main">
      <SearchBooks
        :categories="categories"
        @search-books="getBooks"
        @clear-books="clearBooks"
      />
      <ErrorMessage v-if="error" :error="error" />
      <Loader v-if="loading" />
      <BookList :books="books" v-if="books && !loading" />
    </div>
  </div>
</template>

<script>
import ErrorMessage from "./components/ErrorMessage";
import BookList from "./components/BookList";
import SearchBooks from "./components/SearchBooks";
import Navbar from "./components/Navbar";
import Loader from "./components/Loader";
import axios from "axios";

export default {
  name: "app",
  data() {
    return {
      categories: [],
      books: null,
      loading: false,
      error: null,
    };
  },
  components: {
    ErrorMessage,
    BookList,
    SearchBooks,
    Navbar,
    Loader,
  },
  methods: {
    async getBooks({ category, title }) {
      this.loading = true;
      try {
        const res = await axios.get(
          `https://api.nytimes.com/svc/books/v3/lists/current/${category}.json?api-key=${
            process.env.VUE_APP_NYTIMES_API_KEY
          }`
        );
        this.books = res.data.results.books.filter((bk) =>
          bk.title.toLowerCase().includes(title.toLowerCase())
        );
      } catch (error) {
        this.error = error.message;
        setTimeout(() => {
          this.error = "";
        }, 5000);
      } finally {
        this.loading = false;
      }
    },
    clearBooks() {
      this.books = null;
    },
  },
  async created() {
    try {
      const res = await axios.get(
        `https://api.nytimes.com/svc/books/v3/lists/names.json?api-key=${
          process.env.VUE_APP_NYTIMES_API_KEY
        }`
      );
      this.categories = res.data.results.slice(0, 10);
    } catch (error) {
      this.error = error.message;
    }
  },
};
</script>

<style lang="scss">
@import "./styles/main.scss";

.main {
  margin: auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  @include responsive(laptop) {
    width: 60vw;
  }
}
</style>
