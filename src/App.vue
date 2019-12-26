<!-- <template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    <router-view/>
  </div>
</template> -->

<template>
  <div id="app">
    <Calculator/>
  </div>
</template>

<script>
  import Calculator from "./components/calculator"

  export default {
    name: "app",
    components: {
      Calculator
    }
  }
</script>

<style>
  .has-full-height{
    height: 100vh;
  }
</style>