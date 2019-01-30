<template>
  <div id="app">
    <map2d></map2d>
  </div>
</template>

<script>
import map2d from "./components/map.vue";

export default {
  name: "app",
  components: {
    map2d
  }
};
</script>

<style>
html,
body,
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  height: 100%;
  width: 100%;
  margin: 0;
  padding: 0;
  overflow: hidden;
}
</style>
