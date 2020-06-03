<template>
  <div class="container">
    <h2> About </h2>
    <div>
      <nuxt-link to="/">index</nuxt-link>
      <nuxt-link to="/about">about</nuxt-link>
    </div>
  </div>
</template>

<script>
  export default {
  }
</script>

<style scoped>
  .container {
    min-height: 600px;
    background-color: green;
    border: solid 5px yellow;
  }
</style>
