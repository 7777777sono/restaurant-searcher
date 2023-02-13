<template>
  <div>
    <!--マップ-->
    <l-map style="height: 300px" v-bind:zoom="zoom" v-bind:center="center">
      <!--レイヤーコントロール-->
      <l-control-layers position="topright"></l-control-layers>
      <!--レイヤ設定-->
      <l-tile-layer
        v-bind:visible="tileProvider.visible"
        v-bind:url="tileProvider.url"
        v-bind:attribution="tileProvider.attribution"
        layer-type="base"
      ></l-tile-layer>
      <!--マーカー-->
      <l-marker v-bind:lat-lng="restaurantMarker">
        <l-icon
          v-bind:icon-url="destinationIcon.iconUrl"
          v-bind:icon-size="destinationIcon.iconSize"
          v-bind:icon-anchor="destinationIcon.iconAnchor"
        ></l-icon>
      </l-marker>
      <l-marker :lat-lng="currentLocation">
        <l-icon
          v-bind:icon-url="icon.iconUrl"
          v-bind:icon-size="icon.iconSize"
          v-bind:icon-anchor="icon.iconAnchor"
        ></l-icon>
      </l-marker>
    </l-map>
  </div>
</template>

<script>
import icon from "../assets/2310820_s-removebg-preview (1).png"
import destinationIconImg from "../assets/map_marker-1 (1).png"
import "leaflet/dist/leaflet.css"
import {
  LMap,
  LTileLayer,
  LControlLayers,
  LMarker,
  LIcon,
} from "@vue-leaflet/vue-leaflet"

export default {
  components: {
    LMap,
    LTileLayer,
    LControlLayers,
    LMarker,
    LIcon,
  },
  props: {
    restaurantLat: {
      type: Number,
    },
    restaurantLng: {
      type: Number,
    },
    currentLocation: {
      type: Object,
    },
  },
  inheritAttrs: false,
  data() {
    return {
      center: [this.currentLocation.lat, this.currentLocation.lng], // 地図の中央地点(現在地)
      zoom: 17,
      currentLocationMarker: [
        this.currentLocation.lat,
        this.currentLocation.lng,
      ], // 現在地のマーカー
      restaurantMarker: [this.restaurantLat, this.restaurantLng], // 店の位置のマーカー
      icon: {
        iconUrl: icon,
        iconSize: [27, 27],
        iconAnchor: [16, 37],
      },
      destinationIcon: {
        iconUrl: destinationIconImg,
        iconSize: [42, 42],
        iconAnchor: [16, 37],
      },
      tileProvider: {
        visible: true,
        url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
        attribution:
          '© <a target="_blank" href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      },
    }
  },
}
</script>
