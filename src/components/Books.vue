<template>
  <div class="results">
    <NotFound v-if="books.length === 0" />
    <div v-else v-for="book in books" :key="book.primary_isbn13">
      <BookItem :book="book" />
    </div>
  </div>
</template>

<script>
import BookItem from "./BookItem";
import NotFound from "./NotFound";
export default {
  name: "Books",
  components: {
    BookItem,
    NotFound,
  },
  props: ["books"],
};
</script>

<style scoped lang="scss">
@import "../styles/mixins";
.results {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  @include responsive(tablet) {
    width: 80%;
    display: grid;
    grid-gap: 1rem;
    grid-template-columns: repeat(auto-fill, minmax(250px, auto));
    align-items: stretch;
  }
}
</style>
