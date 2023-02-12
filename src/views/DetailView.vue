<template>
  <div class="restaurant">
    <h3>{{ restaurant.info.name }}</h3>
  </div>
</template>

<script>
export default {
  props: {
    restaurant: {
      type: Object,
    },
  },

  data() {
    return {}
  },
}
</script>
