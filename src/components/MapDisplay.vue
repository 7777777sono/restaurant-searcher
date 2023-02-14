<template>
  <div class="map-zone">
    <!--マップ-->
    <l-map v-bind:zoom="zoom" v-bind:center="center">
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
        <l-popup v-bind:content="destinationIcon.contents"></l-popup>
        <l-icon
          v-bind:icon-url="destinationIcon.iconUrl"
          v-bind:icon-size="destinationIcon.iconSize"
          v-bind:icon-anchor="destinationIcon.iconAnchor"
        ></l-icon>
      </l-marker>
      <l-marker :lat-lng="currentLocation">
        <l-popup v-bind:content="icon.contents"></l-popup>
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
  LPopup,
} from "@vue-leaflet/vue-leaflet"

export default {
  components: {
    LMap,
    LTileLayer,
    LControlLayers,
    LMarker,
    LIcon,
    LPopup,
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
      center: [this.restaurantLat, this.restaurantLng], // 地図の中央地点(店の地点)
      zoom: 15, // 地図をどのくらい拡大するのかを指定する数値
      currentLocationMarker: [
        this.currentLocation.lat,
        this.currentLocation.lng,
      ], // 現在地のマーカー
      restaurantMarker: [this.restaurantLat, this.restaurantLng], // 店の位置のマーカー
      // 現在地におけるアイコンの詳細
      icon: {
        iconUrl: icon,
        iconSize: [23, 23],
        iconAnchor: [0, 0],
        contents: "現在地",
      },
      // 店の場所におけるアイコンの詳細
      destinationIcon: {
        iconUrl: destinationIconImg,
        iconSize: [42, 42],
        iconAnchor: [0, 0],
        contents: "目的地",
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

<style lang="scss">
@mixin min-width($width: 961px) {
  @media screen and (min-width: $width) {
    @content;
  }
}

@mixin min-max-width($min: 521px, $max: 960px) {
  @media screen and (min-width: $min) and (max-width: $max) {
    @content;
  }
}

@mixin max-width($width: 520px) {
  @media screen and (max-width: $width) {
    @content;
  }
}

.map-zone {
  padding-right: 50px;
  padding-left: 50px;
}

@include max-width(520px) {
  .map-zone {
    height: 200px;
  }
}

@include min-max-width(521px, 960px) {
  .map-zone {
    height: 300px;
  }
}

@include min-width(961px) {
  .map-zone {
    height: 400px;
  }
}
</style>
