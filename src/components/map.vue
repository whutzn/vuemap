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

const DATA = [
  [0.707,0.313],
  [0.699,0.521],
  [0.707,0.313]
]

export default {
  name: 'Map2d',
  data () {
    return {
      map: null,
      heatMap: null,
      movingMarker: null,
      durationTime: 4000,
      coords: ''
    }
  },
  mounted () {
    this.initMap(), this.addHeatMap(), this.addRoute()
  },
  methods: {
    initMap () {
      let x = 820, y = 420;
      this.map = L.map('map', {
        center: [x, y], // 中心点坐标
        zoom: 0, // 放大级别
        crs: L.CRS.Simple,
        minZoom: -4,
        maxZoom: 4,
        maxBounds: [[0,0],[x*2,y*2]]
      })
      let imageUrl = require('../assets/data.svg')// 背景地图
      let imageBounds = [[x/2, y/2], [x+x/2, y+y/2]]// 尺寸

      L.imageOverlay(imageUrl, imageBounds).addTo(this.map)

      this.map.fitBounds(imageBounds)

      this.map.setZoom(2)// 调整缩放级别

      // this.map.setMaxBounds([[0,0],[820*2,425*2]])

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
        console.log('point', 'lat: '+e.latlng.lat,'lng: '+e.latlng.lng)
        this.coords =e.latlng.lat+','+e.latlng.lng
        // this.heatMap.addLatLng(e.latlng)
      })
    },
    addRoute () {
      // 轨迹坐标
      let latlngs = [
      ]
      DATA.forEach(element => {
        let coord = this.map.layerPointToLatLng(L.point(element[0]*822, element[1]*425))
        latlngs.push([coord.lat,coord.lng])
      });
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

      this.movingMarker = L.marker(latlngs[0], {
            icon: myIcon
          }).addTo(this.map)

      for (let index = 1; index < latlngs.length; index++) {
        if (index == 1) {
          this.movingMarker.slideTo(latlngs[index], {
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
  },
  watch: {
    'coords': function () {
      this.$emit('getMessage',this.coords);
    }
  },
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
