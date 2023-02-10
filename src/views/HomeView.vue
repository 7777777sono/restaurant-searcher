<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <HelloWorld msg="Welcome to Your Vue.js App" />
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue"
import VueGeolocation from "vue3-geolocation"

export default {
  name: "HomeView",
  components: {
    HelloWorld,
  },
  data() {
    return {
      current_location: {}, // 現在地を取得するオブジェクト
    }
  },
  methods: {
    getLocation: async function () {
      await VueGeolocation.getLocation()
        .then((coordinates) => {
          this.current_location = {
            lat: coordinates.lat, //緯度
            lng: coordinates.lng, //経度
          }
        })
        .catch((error) => {
          console.log(error)
        })
    },
  },
}
</script>
