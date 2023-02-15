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
    <div class="input-zone">
      <h3>キーワードを入力してください。</h3>
      <input class="text-input" type="text" v-model="keyword" />
    </div>
    <div class="search-button">
      <button v-on:click="search">検索</button>
    </div>
  </div>
</template>

<script>
export default {
  emits: ["search"],
  data() {
    return {
      radius: 1, // 検索半径(km単位)
      radiusMin: 0, // 検索半径の最小値
      radiusMax: 3, // 検索半径の最大値
      keyword: "", // 検索する際のキーワード
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
      this.$emit("search", this.radius, this.keyword)
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
    margin-bottom: 20px;
    padding-right: 10px;
    padding-left: 10px;
    h3 {
      margin-top: 20px;
    }
    h4 {
      margin-top: 0;
    }
    .text-input {
      width: 50%;
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
    border-radius: 0.25rem;
    margin-bottom: 20px;
    padding: 5px 20px;
    font-size: 20px;
    cursor: pointer;
    font-family: "Merriweather", serif;
    font-family: "Noto Serif HK", serif;
    font-family: "Noto Serif JP", serif;
    font-family: "PT Serif", serif;
    font-family: "Shippori Mincho", serif;
  }
  @include min-width(961px) {
    button:hover {
      color: black;
    }
  }
}
</style>
