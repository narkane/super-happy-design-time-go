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
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#shirt {
  position: absolute;
  top: 0;
  left: 0;
  z-index: -20;
}
</style>
