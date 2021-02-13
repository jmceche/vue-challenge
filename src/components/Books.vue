<template>
  <div class="container">
    <div class="card">
      <div class="heading">
        <h4 class="title">NYTimes Books</h4>
      </div>
      <div class="content">
        <!-- ADD CONTENT HERE -->
        <form @submit.prevent="getBooks" class="search">
          <select v-model="selected" name="categories" id="categories">
            <option
              v-for="category in categories"
              :value="category.list_name_encoded"
              :key="category.list_name_encoded"
              >{{ category.display_name }}</option
            >
          </select>
          <input
            type="text"
            class="form-control"
            placeholder="Search by title"
            v-model="text"
          />
          <button type="submit">Search</button>
        </form>
        <div v-for="book in books" :key="book.primary_isbn13">
          <BookItem :book="book" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import BookItem from "./BookItem";
export default {
  name: "Books",
  props: ["categories"],
  components: {
    BookItem,
  },
  data() {
    return {
      text: "",
      selected: "",
      books: [],
    };
  },
  methods: {
    async getBooks(e) {
      const res = await fetch(
        `https://api.nytimes.com/svc/books/v3/lists/current/${
          this.selected
        }.json?api-key=${process.env.VUE_APP_NYTIMES_API_KEY}`
      );
      const data = await res.json();
      const result = this.text
        ? data.results.books.filter((bk) =>
            bk.title.toLowerCase().includes(this.text.toLowerCase())
          )
        : [{ title: "No Result found" }];
      this.books = result;
    },
  },
};
</script>

<style scoped lang="scss">
.container {
  z-index: 1;
  margin: 36px auto;
  max-width: 826px;
  background-color: white;
}

.card {
  box-shadow: 0 2px 3px rgba(10, 10, 10, 0.1), 0 0 0 1px rgba(10, 10, 10, 0.1);
  padding: 24px;
}

.list {
  > li {
    &:not(:last-child) {
      margin-bottom: 18px;
    }
    > a {
      color: #0a5b8c;
      display: block;
      margin-bottom: 6px;
    }

    > span {
      color: rgba(#3b4242, 0.7);
      font-size: 12px;
    }
  }
}

.btn {
  color: #fff;
  cursor: pointer;
  background-color: #117a8b;
  border: 1px solid transparent;
  padding: 6px 12px;
  border-radius: 6px;
  transition: all 0.1s ease-in;
  &:hover {
    background-color: #138496;
    border-color: #117a8b;
  }
}

.heading {
  margin-bottom: 12px;

  .title {
    font-size: 18px;
    font-weight: 600;
  }
}
.search {
  margin-bottom: 24px;
  .form-control {
    background-color: transparent;
    border: none;
    border-bottom: 1px solid #ced4da;
    border-radius: 0;
    outline: 0;
    box-shadow: none;
    padding: 6px 0;
    width: 100%;
  }
}
</style>
