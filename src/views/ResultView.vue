<template>
  <div v-if="results.length > 0" class="results-zone">
    <div
      v-for="(result, index) in results"
      v-bind:key="index"
      class="shop-zone"
    >
      <img v-bind:src="result.info.photo.pc.l" />
      <h3>{{ result.info.name }}</h3>
      <h3>アクセス: {{ result.info.mobile_access }}</h3>
      <h3>
        距離: およそ{{ Math.round(Math.round(result.distance * 10)) / 10 }}km
      </h3>
      <button v-on:click="screenTransition(index)">詳細</button>
    </div>
  </div>
  <div class="link-zone">
    <h4>以下のリンクよりお戻りください。</h4>
    <router-link to="/">Home</router-link>
  </div>
</template>

<script>
export default {
  props: {
    results: {
      type: Array,
      default: () => [],
      required: true,
    },
  },
  emits: ["detail"],
  inheritAttrs: false,
  methods: {
    // 詳細画面に遷移するための関数
    screenTransition: function (index) {
      this.$emit("detail", index)
      this.$router.push("/detail")
    },
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

.results-zone {
  padding-left: 20px;
  padding-right: 20px;
  display: flex;
  flex-wrap: wrap;
  .shop-zone {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
    overflow: hidden;
    border-radius: 8px;
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
    margin-right: 10px;
    margin-left: 10px;
    margin-bottom: 20px;
    font-family: "Merriweather", serif;
    font-family: "Noto Serif HK", serif;
    font-family: "Noto Serif JP", serif;
    font-family: "PT Serif", serif;
    font-family: "Shippori Mincho", serif;
    @include max-width(520px) {
      width: calc(100%);
    }
    @include min-max-width(521px, 960px) {
      width: calc(50% - 20px);
    }
    @include min-width(961px) {
      width: calc(25% - 20px);
    }
    h3 {
      padding-left: 20px;
      padding-right: 20px;
    }
    img {
      padding-top: 0;
      height: 200px;
      width: 100%;
      margin-bottom: 10px;
    }
    button {
      margin-bottom: 20px;
      width: 30%;
      font-size: 20px;
      padding-top: 5px;
      padding-bottom: 5px;
      background: #09c;
      border: #09c;
      color: white;
      border-radius: 0.25rem;
      cursor: pointer;
      font-family: "Merriweather", serif;
      font-family: "Noto Serif HK", serif;
      font-family: "Noto Serif JP", serif;
      font-family: "PT Serif", serif;
      font-family: "Shippori Mincho", serif;
    }
  }
  @include min-width(961px) {
    .shop-zone:hover {
      transform: translateY(-3px);
      box-shadow: 0 7px 14px rgba(50, 50, 93, 0.1),
        0 3px 6px rgba(0, 0, 0, 0.08);
      transition: all 0.5s;
      button:hover {
        color: black;
      }
    }
  }
}

.link-zone {
  padding-right: 20px;
  padding-left: 20px;
  font-family: "Merriweather", serif;
  font-family: "Noto Serif HK", serif;
  font-family: "Noto Serif JP", serif;
  font-family: "PT Serif", serif;
  font-family: "Shippori Mincho", serif;
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
