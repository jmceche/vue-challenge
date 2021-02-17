<template>
  <div class="error">
    <h3>Error: {{ error }}</h3>
  </div>
</template>

<script>
export default {
  props: {
    error: String,
  },
}
</script>

<style lang="scss" scoped>
.error {
  background: #ff4343;
  padding: 1rem;
  width: 50%;

  h3 {
    font-weight: bold;
    text-align: center;
  }
}
</style>
