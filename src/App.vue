<template>
  <div id="app">
    <div class="navbar">
      <Books :categories="categories" />
    </div>
  </div>
</template>

<script>
import Books from "./components/Books.vue";

export default {
  name: "app",
  data() {
    return {
      categories: [],
    };
  },
  components: {
    Books,
  },
  methods: {},
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

.navbar {
  background-color: orange;
  width: 100%;
  height: 96px;
  position: absolute;
  z-index: 0;
}
</style>
