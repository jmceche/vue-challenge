<template>
  <form @submit.prevent="getBooks">
    <select
      v-model="category"
      name="categories"
      id="categories"
      @change="clearSearch"
      :disabled="!categories.length"
    >
      <option default value="">--Select a Category--</option>
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
      v-model="title"
      :disabled="!category"
      required
    />
  </form>
</template>

<script>
export default {
  name: "SearchBooks",
  props: {
    categories: Array,
  },
  data() {
    return {
      category: "",
      title: "",
    };
  },
  methods: {
    getBooks() {
      this.$emit("search-books", {
        category: this.category,
        title: this.title,
      });
    },
    clearSearch() {
      this.title = "";
      this.$emit("clear-books");
    },
  },
};
</script>

<style lang="scss" scoped>
@import "../styles/mixins";

@mixin inputs {
  width: 100%;
  background: #fff;
  color: #000;
  padding: 0.5rem;
  border-radius: 5px;
  font-size: 1rem;

  @include responsive(tablet) {
    font-size: 1.4rem;
  }
}
form {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 4rem;
  @include responsive(tablet) {
    max-width: 500px;
  }
}

select {
  margin-bottom: 0.5rem;
  @include inputs;
}
input {
  @include inputs;
}
</style>
