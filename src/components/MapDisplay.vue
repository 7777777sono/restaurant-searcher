<template>
  <div>
    <!--マップ-->
    <l-map style="height: 600px" v-bind:zoom="zoom" v-bind:center="center">
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
      <l-marker v-bind:lat-lng="marker"></l-marker>
    </l-map>
  </div>
</template>

<script>
import "leaflet/dist/leaflet.css"
import {
  LMap,
  LTileLayer,
  LControlLayers,
  LMarker,
} from "@vue-leaflet/vue-leaflet"

export default {
  components: {
    LMap,
    LTileLayer,
    LControlLayers,
    LMarker,
  },

  data() {
    return {
      center: [35.681, 139.763],
      zoom: 14,
      marker: [35.681, 139.763],
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
