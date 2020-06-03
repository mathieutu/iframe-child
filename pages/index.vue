<template>
  <div class="container">
    <h2> Index </h2>
    {{iframe.params}}
    <button v-if="!iframe.params.modal" @click="iframe.emit('modal', 'description')">Open Modal</button>
    <button v-else @click="iframe.emit('modal', null)">Close Modal</button>
    <div>
      <nuxt-link to="/">index</nuxt-link>
      <nuxt-link to="/about">about</nuxt-link>
    </div>
  </div>
</template>

<script>
  export default {
    inject: ['iframe'],
  }
</script>

<style scoped>
  .container {
    min-height: 400px;
    background-color: orangered;
    border: solid 5px yellow;
  }
</style>
