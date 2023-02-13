<template>
  <h1 class="title">Restaurant Searcher</h1>
  <router-view
    v-bind:results="results"
    v-bind:restaurant="detailRestaurant"
    v-on:detail="detailDisplay"
    v-on:search="searchResturant"
  />
</template>

<script>
import VueGeolocation from "vue3-geolocation"

export default {
  data() {
    return {
      API_KEY: "3ea416f7ceb2e235", // HOTPPEPERのAPIキー
      current_location: {}, // 現在地を取得するオブジェクト
      url: "http://webservice.recruit.co.jp/hotpepper/gourmet/v1/?key=", // APIのURL(APIキー前までの)
      searchRange: 1, // HOTPPEPER APIの検索範囲(ある地点からの範囲内のお店の検索)を決める変数
      format: "json", // レスポンス形式の指定
      restaurants: [], // 取得した店の情報を格納する
      results: [], // 検索結果を格納する連想配列
      detailRestaurant: {}, // 店舗詳細を見たい店舗の情報を格納するオブジェクト
    }
  },
  methods: {
    // 現在地を取得する関数
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
    // 店の情報を取得する関数
    getRestaurant: async function (radius) {
      // 検索半径300m以内なら
      if (radius * 1000 <= 300) {
        this.searchRange = 1
      }
      // 検索半径500m以内なら
      else if (radius * 1000 <= 500) {
        this.searchRange = 2
      }
      // 検索半径1000m以内なら
      else if (radius * 1000 <= 1000) {
        this.searchRange = 3
      }
      // 検索半径2000m以内なら
      else if (radius * 1000 <= 2000) {
        this.searchRange = 4
      }
      // 検索半径3000m以内なら
      else if (radius * 1000 <= 3000) {
        this.searchRange = 5
      }
      const data = await fetch(
        this.url +
          this.API_KEY +
          "&lat=" +
          this.current_location.lat +
          "&lng=" +
          this.current_location.lng +
          "&range=" +
          this.searchRange +
          "&format=" +
          this.format
      )
      const json = await data.json()
      this.restaurants = json.results.shop
    },
    // 検索結果を取得する関数
    getResults: function () {},

    // とある2点間の距離(km単位)を計算する関数
    distance: function (lat1, lng1, lat2, lng2) {
      const R = Math.PI / 180
      const EARTH_RADIUS = 6371
      lat1 *= R
      lng1 *= R
      lat2 *= R
      lng2 *= R
      return (
        EARTH_RADIUS *
        Math.acos(
          Math.cos(lat1) * Math.cos(lat2) * Math.cos(lng2 - lng1) +
            Math.sin(lat1) * Math.sin(lat2)
        )
      )
    },
    searchResturant: async function (radius) {
      this.results = []
      await this.getLocation()
      await this.getRestaurant(radius)
      for (let i = 0; i < this.restaurants.length; i++) {
        // 検索半径なら検索結果とする
        if (
          radius >=
          this.distance(
            this.current_location.lat,
            this.current_location.lng,
            this.restaurants[i].lat,
            this.restaurants[i].lng
          )
        ) {
          this.results.push({
            info: this.restaurants[i],
            distance: this.distance(
              this.current_location.lat,
              this.current_location.lng,
              this.restaurants[i].lat,
              this.restaurants[i].lng
            ),
          })
        }
      }
    },
    // 店舗詳細を画面に出力するための関数
    detailDisplay: function (index) {
      this.detailRestaurant = this.results[index]
    },
  },
}
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Merriweather:ital,wght@1,300&family=Noto+Serif+HK:wght@200&family=Noto+Serif+JP:wght@200&family=PT+Serif:ital@1&family=Shippori+Mincho&display=swap");

.title {
  margin-top: 10px;
  padding-right: 20px;
  padding-left: 20px;
  font-family: "Merriweather", serif;
  font-family: "Noto Serif HK", serif;
  font-family: "Noto Serif JP", serif;
  font-family: "PT Serif", serif;
  font-family: "Shippori Mincho", serif;
}
</style>
