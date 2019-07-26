<template>
  <div id="app">
    <div id="mainmap">
    <map2d @getMessage="getVal"></map2d>
    </div>
    <div id="info">
      <p>子组件输入的值:{{coords}}</p>
    </div>
  </div>
</template>

<script>
import map2d from './components/map.vue'

export default {
  name: 'App',
  data() {
    return {
      coords: ''
    }
  },
  components: {
    map2d
  },
  methods:{
    getVal(msg){
      this.coords = msg
      // alert(this.coords)
    }
  }
}
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
#info {
  height: 20%;
  width: 100%;
  text-align: center;
}
#mainmap {
  height: 80%;
  width: 100%;
}
</style>
