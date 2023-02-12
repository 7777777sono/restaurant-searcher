<template>
  <div class="restaurant">
    <img v-bind:src="restaurant.info.photo.pc.l" />
    <div>
      <h2>{{ restaurant.info.name }}</h2>
      <h3>{{ restaurant.info.name_kana }}</h3>
    </div>
    <h3>{{ restaurant.info.genre.name }}</h3>
    <h3>住所: {{ restaurant.info.address }}</h3>
    <h3>アクセス: {{ restaurant.info.mobile_access }}</h3>
    <div>
      <h3>営業時間: {{ restaurant.info.open }}</h3>
      <h3>定休日: {{ restaurant.info.close }}</h3>
    </div>
  </div>
  <router-link to="/">Home</router-link>
</template>

<script>
export default {
  props: {
    restaurant: {
      type: Object,
    },
  },

  inheritAttrs: false,

  data() {
    return {}
  },
}
</script>
