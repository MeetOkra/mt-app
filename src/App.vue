<template>
  <div id="app">
    <app-header :poiInfo="poiInfo"></app-header>

    <app-nav :ratingNum="ratingNum"></app-nav>

    <keep-alive>
      <router-view></router-view>
    </keep-alive>
  </div>
</template>

<script>
  import Header from "./components/header/Header";
  import Nav from "./components/nav/Nav";

  export default {
    name: 'App',
    components: {
      "app-header": Header,
      "app-nav": Nav
    },
    data() {
      return {
        poiInfo: {},
        ratingNum: 0
      }
    },
    created() {
      fetch("/api/goods")
        .then(res => res.json())
        .then(response => {
          console.log(response)
          if (response.code === 0) {
            this.poiInfo = response.data.poi_info;
          }
        })
      fetch("/api/ratings")
        .then(res => res.json())
        .then(response => {
          console.log(response)
          if (response.code === 0) {
            this.ratingNum = response.data.comment_num
          }
        })
    }
  }
</script>

<style>
</style>
