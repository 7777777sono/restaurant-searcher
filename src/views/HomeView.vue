<template>
  <div class="search-zone">
    <div class="input-zone">
      <h3>検索範囲をkm単位で入力してください。(3kmまで)</h3>
      <h4>※500mなら0.5</h4>
      <input
        type="number"
        v-model="radius"
        v-on:change="radiusJudge"
        max="3"
        min="0"
      />
    </div>
    <div class="search-button">
      <button v-on:click="search">検索</button>
    </div>
  </div>
  <div v-if="results.length > 0" class="results-zone">
    <div
      v-for="(result, index) in results"
      v-bind:key="index"
      v-on:click="screenTransition(index)"
      class="shop-zone"
    >
      <img v-bind:src="result.info.photo.pc.l" />
      <h3>{{ result.info.name }}</h3>
      <h3>アクセス: {{ result.info.mobile_access }}</h3>
    </div>
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
  inheritAttrs: false,
  emits: ["search", "detail"],
  data() {
    return {
      radius: 1, // 検索半径(km単位)
      radiusMin: 0, // 検索半径の最小値
      radiusMax: 3, // 検索半径の最大値
    }
  },
  methods: {
    // 検索半径が範囲内を判別し、範囲外なら範囲の最大値または最小値に変更
    radiusJudge: function () {
      if (this.radius <= this.radiusMin) {
        this.radius = this.radiusMin
      } else if (this.radius >= this.radiusMax) {
        this.radius = this.radiusMax
      }
    },
    // 検索を行うための関数
    // App.vueにsearchというイベントを発火し、App.vueで店の情報の取得を行う。
    search: function () {
      this.radiusJudge()
      this.$emit("search", this.radius)
    },
    // 詳細画面に遷移するための関数
    screenTransition: function (index) {
      this.$emit("detail", index)
      this.$router.push("/detail")
    },
  },
}
</script>

<style lang="scss">
@mixin min-width($width: 481px) {
  @media screen and (min-width: $width) {
    @content;
  }
}

@mixin min-max-width($min: 481px, $max: 800px) {
  @media screen and (min-width: $min) and (max-width: $max) {
    @content;
  }
}

@mixin max-width($width: 480px) {
  @media screen and (max-width: $width) {
    @content;
  }
}

@import url("https://fonts.googleapis.com/css2?family=Merriweather:ital,wght@1,300&family=Noto+Serif+HK:wght@200&family=Noto+Serif+JP:wght@200&family=PT+Serif:ital@1&family=Shippori+Mincho&display=swap");

.search-zone {
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding-right: 20px;
  padding-left: 20px;
  font-family: "Merriweather", serif;
  font-family: "Noto Serif HK", serif;
  font-family: "Noto Serif JP", serif;
  font-family: "PT Serif", serif;
  font-family: "Shippori Mincho", serif;
  .input-zone {
    overflow: hidden;
    border-radius: 8px;
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
    padding-right: 10px;
    padding-left: 10px;
    h3 {
      margin-top: 20px;
    }
    h4 {
      margin-top: 0;
    }
    input {
      width: 10%;
      font-size: 15px;
      border-top: none;
      border-left: none;
      border-right: none;
      border-bottom: solid 1px rgb(176, 174, 174);
      margin-bottom: 20px;
    }
    input:focus {
      border-top: none;
      border-left: none;
      border-right: none;
      border-bottom: solid 1px #09c;
      outline: none;
    }
  }
  button {
    background: #09c;
    border: #09c;
    color: white;
    border-radius: 100vh;
    margin-top: 10px;
    margin-bottom: 20px;
    padding-right: 10px;
    padding-left: 10px;
    font-size: 17px;
    cursor: pointer;
    font-family: "Merriweather", serif;
    font-family: "Noto Serif HK", serif;
    font-family: "Noto Serif JP", serif;
    font-family: "PT Serif", serif;
    font-family: "Shippori Mincho", serif;
  }
  @include min-width(520px) {
    button:hover {
      color: black;
    }
  }
}

.results-zone {
  padding-left: 20px;
  padding-right: 20px;
  display: flex;
  flex-wrap: wrap;
  font-family: "Merriweather", serif;
  font-family: "Noto Serif HK", serif;
  font-family: "Noto Serif JP", serif;
  font-family: "PT Serif", serif;
  font-family: "Shippori Mincho", serif;
  .shop-zone {
    overflow: hidden;
    border-radius: 8px;
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
    margin-right: 10px;
    margin-left: 10px;
    margin-bottom: 20px;
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
  }
  @include min-width(521px) {
    .shop-zone:hover {
      transform: translateY(-3px);
      box-shadow: 0 7px 14px rgba(50, 50, 93, 0.1),
        0 3px 6px rgba(0, 0, 0, 0.08);
      transition: all 0.5s;
      cursor: pointer;
    }
  }
}
</style>
