<template>
  <div class="card">
    <a
      :href="
        `https://www.google.com/search?q=${capitalizedTitle}+${bookItem.author}`
      "
      target="_blank"
    >
      <div class="author">
        <h2>
          {{ capitalizedTitle }}
        </h2>
        <h4>
          {{ bookItem.author }}
        </h4>
      </div>
      <p class="description">{{ bookItem.description }}</p>
    </a>
  </div>
</template>

<script>
export default {
  name: "BookItem",
  props: {
    bookItem: Object,
  },
  computed: {
    capitalizedTitle() {
      return this.bookItem.title.toLowerCase();
    },
  },
};
</script>

<style lang="scss" scoped>

.card {
  background: #ace6f6;
  border: 1px solid lightgray;
  border-radius: 10px;
  padding: 1rem 0.5rem;
  margin: 0.5rem auto;
  height: 100%;
  width: 90vw;

  &:hover {
    box-shadow: 2px 2px 5px gray;
  }

  @include responsive(tablet) {
    margin: 0;
    width: auto;
    max-width: 90vw;
  }

  > a {
    text-decoration: none;
    color: inherit;
    display: flex;
    flex-direction: column;
    height: 100%;

    :visited {
      color: inherit;
    }

    h2 {
      text-transform: capitalize;
      font-weight: bold;
      font-size: 1.3rem;
    }

    h4 {
      font-style: italic;
      font-size: 1.1rem;
    }
    .description {
      margin-top: 1rem;
    }
  }
}
</style>
