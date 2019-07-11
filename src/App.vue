<template>
  <div id="app">
    <!-- USE FOLLOWING TWO LINES TO PASS IN PRODUCT IMG AND ID FOR CREATING TEMPLATE -->
    <img src="./assets/t-shirts-1.jpg" id="shirt" />
    <HelloWorld prodID="42" />
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: "app",
  components: {
    HelloWorld
  }
};
</script>

<style>
</style>
