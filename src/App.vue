<template>
  <div id="app">
    <Navbar />
    <div class="main">
      <SearchBooks :categories="categories" @search-books="getBooks" />
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
      const res = await fetch(
        `https://api.nytimes.com/svc/books/v3/lists/current/${category}.json?api-key=${
          process.env.VUE_APP_NYTIMES_API_KEY
        }`
      );
      const data = await res.json();
      this.loading = false;
      this.books = data.results.books.filter((bk) =>
        bk.title.toLowerCase().includes(title.toLowerCase())
      );
    },
  },
  async created() {
    const res = await fetch(
      `https://api.nytimes.com/svc/books/v3/lists/names.json?api-key=${
        process.env.VUE_APP_NYTIMES_API_KEY
      }`
    );
    const data = await res.json();
    this.categories = data.results.slice(0, 10);
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
