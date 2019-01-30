<template>
  <div id="map">
  </div>
</template>

<script>
import L from "leaflet";

const MAP_LAYER = L.tileLayer(
    "http://webrd01.is.autonavi.com/appmaptile?lang=zh_cn&size=1&scale=1&style=8&x={x}&y={y}&z={z}"
  );

export default {
  name: "map2d",
  data() {
    return {
      map: null
    };
  },
  mounted() {
    this.initMap();
  },
  methods: {
    initMap() {
      this.map = L.map("map", {
        minZoom: 3,
        maxZoom: 18,
        attributionControl: false,
        layers: [MAP_LAYER],
        zoomControl: false,
        center: [31.23694, 121.45361],
        zoom: 15
      });
    }
  }
};
</script>

<style src='../assets/leaflet.css'></style>

<style scoped>
#map {
  height: 100%;
  width: 100%;
  margin: 0;
  padding: 0;
}
</style>

