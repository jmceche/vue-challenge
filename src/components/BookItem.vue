<template>
  <div>
    <p>
      <a
        :href="`https://www.google.com/search?q=${book.title}+${book.author}`"
        target="_blank"
      >
        {{ book.title }} - {{ book.author }}
      </a>
    </p>
  </div>
</template>

<script>
export default {
  name: "BookItem",
  props: ["book"],
};
</script>

<style></style>
