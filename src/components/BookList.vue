<template>
  <div class="results">
    <NotFound v-if="books.length === 0" />
    <div class="books" v-else>
      <BookItem
        :book-item="book"
        v-for="book in books"
        :key="book.primary_isbn13"
      />
    </div>
  </div>
</template>

<script>
import BookItem from "./BookItem";
import NotFound from "./NotFound";
export default {
  name: "BookList",
  components: {
    BookItem,
    NotFound,
  },
  props: {
    books: Array,
  },
};
</script>

<style scoped lang="scss">
@import "../styles/mixins";
.results {
  width: 100%;
}
.books {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin: auto;
  @include responsive(tablet) {
    width: 80%;
    display: grid;
    grid-gap: 1rem;
    grid-template-columns: repeat(auto-fit, minmax(250px, auto));
    align-items: stretch;
  }
}
</style>
