<template>
  <div id="app">
    <app-header :poiInfo="poiInfo"></app-header>

    <app-nav></app-nav>

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
        poiInfo: {}
      }
    },
    created() {
      fetch("/api/goods")
        .then(res => res.json())
        .then(response => {
          if (response.code === 0) {
            this.poiInfo = response.data.poi_info;
          }
        })
    }
  }
</script>

<style>
</style>
