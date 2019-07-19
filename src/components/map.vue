<template>
  <div class="map_main">
    <div class="control_panel">
      <button @click="this.pasueMove">暂停</button>
      <button @click="this.controlSpeed">2X</button>
      <button >继续</button>
    </div>
    <div id="map"></div>
  </div>
</template>

<script>
import L from 'leaflet'
import 'leaflet.heat'
import 'leaflet.marker.slideto'

export default {
  name: 'Map2d',
  data () {
    return {
      map: null,
      heatMap: null,
      movingMarker: null,
      durationTime: 4000,
    }
  },
  mounted () {
    this.initMap(), this.addHeatMap(), this.addRoute()
  },
  methods: {
    initMap () {
      this.map = L.map('map', {
        center: [410, 210], // 中心点坐标
        zoom: 0, // 放大级别
        crs: L.CRS.Simple,
        minZoom: -4,
        maxZoom: 4
      })
      let imageUrl = require('../assets/data.svg')// 背景地图
      let imageBounds = [[0, 0], [822.051, 425.199]]// 尺寸

      L.imageOverlay(imageUrl, imageBounds).addTo(this.map)

      this.map.fitBounds(imageBounds)

      this.map.setZoom(2)// 调整缩放级别

      this.map.on('zoomend', e => {
        // 打印当前缩放级别
        console.log('zoom', this.map.getZoom())
      })
    },
    addHeatMap () {
      this.heatMap = L.heatLayer(
        [], // 热力图坐标
        { radius: 25, // 热力图半径
          gradient: { 0.4: 'blue', 0.65: 'lime', 1: 'red' } // 设置颜色渐变
        }
      )

      this.map.addLayer(this.heatMap)

      this.map.on('click', e => {
        // 打印当前点击坐标
        console.log('point', e.latlng)
        // 每次点击后将坐标写入到热力图图层
        this.heatMap.addLatLng(e.latlng)
      })
    },
    addRoute () {
      // 轨迹坐标
      let latlngs = [
        [470.5, 174.5],
        [343.5, 174.75],
        [343, 222]
      ]
      L.polyline(latlngs, {
        color: 'red', // 颜色
        weight: 3, // 线宽
        dashArray: '5 5'// 虚线设置,去掉即为实线
      }).addTo(this.map)

      let myIcon = L.icon({
        iconUrl: require('../assets/people.png'), // 图标路径
        iconSize: [48, 48], // 图标尺寸
        iconAnchor: [24, 48]// 图标偏移
      })

      let movingMarker = null, durationTime = 5000;

      for (let index = 1; index < latlngs.length; index++) {
        if (index == 1) {
          this.movingMarker = L.marker(latlngs[index - 1], {
            icon: myIcon
          }).addTo(this.map).slideTo(latlngs[index], {
            duration: this.durationTime// 动画时间
          })
        } else {
          setTimeout(() => {
            this.movingMarker.slideTo(latlngs[index], {
              duration: this.durationTime// 动画时间
            })
          }, this.durationTime)
        }
      }
    },
    pasueMove() {
      this.movingMarker.slideCancel()
    },
    controlSpeed() {

    }
  }
}
</script>

<style src='../assets/leaflet.css'></style>

<style>
.map_main {
  height: 100%;
  width: 100%;
}
.control_panel {
  position: absolute;
  z-index: 999;
}
#map {
  height: 100%;
  width: 100%;
  margin: 0;
  padding: 0;
  background-color: #041529
}
.leaflet-bar {
  display: none !important;
}

.leaflet-control-attribution {
  display: none !important;
}

</style>
