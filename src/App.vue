<template>
  <div id="app">
    <Navbar />
    <div class="main">
      <SearchBooks
        :categories="categories"
        @search-books="getBooks"
        @clear-books="clearBooks"
      />
      <Loader v-if="loading" />
      <Books :books="books" v-if="books && !loading" />
    </div>
  </div>
</template>

<script>
import Books from "./components/Books";
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
    };
  },
  components: {
    Books,
    SearchBooks,
    Navbar,
    Loader,
  },
  methods: {
    async getBooks({ category, title }) {
      this.loading = true;
      const res = await axios.get(
        `https://api.nytimes.com/svc/books/v3/lists/current/${category}.json?api-key=${
          process.env.VUE_APP_NYTIMES_API_KEY
        }`
      );
      this.loading = false;
      this.books = res.data.results.books.filter((bk) =>
        bk.title.toLowerCase().includes(title.toLowerCase())
      );
    },
    clearBooks() {
      this.books = null;
    },
  },
  async created() {
    const res = await axios.get(
      `https://api.nytimes.com/svc/books/v3/lists/names.json?api-key=${
        process.env.VUE_APP_NYTIMES_API_KEY
      }`
    );
    this.categories = res.data.results.slice(0, 10);
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
