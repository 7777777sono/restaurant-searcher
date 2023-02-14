<template>
  <div class="restaurant">
    <img v-bind:src="restaurant.info.photo.pc.l" />
    <div class="text-zone">
      <h1>{{ restaurant.info.name }}</h1>
      <h3>ジャンル: {{ restaurant.info.genre.name }}</h3>
      <h3>住所: {{ restaurant.info.address }}</h3>
      <h3>アクセス: {{ restaurant.info.mobile_access }}</h3>
      <h3>営業時間: {{ restaurant.info.open }}</h3>
      <h3>定休日: {{ restaurant.info.close }}</h3>
    </div>
  </div>
  <MapDisplay
    v-bind:restaurantLat="restaurant.info.lat"
    v-bind:restaurantLng="restaurant.info.lng"
    v-bind:currentLocation="currentLocation"
  ></MapDisplay>
  <div class="link-zone">
    <h4>以下のリンクよりお戻りください。</h4>
    <router-link to="/">Home</router-link>
  </div>
</template>

<script>
import MapDisplay from "@/components/MapDisplay.vue"

export default {
  components: { MapDisplay },
  props: {
    restaurant: {
      type: Object,
    },
    currentLocation: {
      type: Object,
    },
  },
  inheritAttrs: false,
  mounted() {
    window.scrollTo(0, 0)
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

@import url("https://fonts.googleapis.com/css2?family=Merriweather:ital,wght@1,300&family=Noto+Serif+HK:wght@200&family=Noto+Serif+JP:wght@200&family=PT+Serif:ital@1&family=Shippori+Mincho&display=swap");

.restaurant,
.link-zone {
  padding-right: 20px;
  padding-left: 20px;
  font-family: "Merriweather", serif;
  font-family: "Noto Serif HK", serif;
  font-family: "Noto Serif JP", serif;
  font-family: "PT Serif", serif;
  font-family: "Shippori Mincho", serif;
}
.restaurant {
  @include min-width(521px) {
    display: flex;
    align-items: center;
    justify-content: flex-start;
  }
  @include max-width(520px) {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    .text-zone {
      padding-left: 25px;
      h1 {
        font-size: 20px;
      }
      h3 {
        font-size: 13px;
      }
    }
    img {
      height: 210px;
      width: 280px;
    }
  }
  @include min-max-width(521px, 960px) {
    .text-zone {
      padding-left: 30px;
      h1 {
        font-size: 25px;
      }
      h3 {
        font-size: 15px;
      }
    }
    img {
      height: 210px;
      width: 280px;
    }
  }
  @include min-width(961px) {
    .text-zone {
      padding-left: 50px;
    }
    img {
      height: 300px;
      width: 400px;
    }
  }
}
.link-zone {
  margin-top: 20px;
  a {
    color: black;
  }
  @include min-width(961px) {
    a:hover {
      color: #09c;
    }
  }
}
</style>
