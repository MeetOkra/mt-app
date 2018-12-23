<template>
  <div class="main-wrapper" :style="detail_bg">
    <div class="icon" :style="head_bg"></div>
    <span class="name">{{poiInfo.name}}</span>

    <!-- 星级评价 -->
    <div class="score">
      <!--<app-star :score="poiInfo.wm_poi_score"></app-star>-->
      <span>{{poiInfo.wm_poi_score}}</span>
    </div>

    <p class="tip">
      {{poiInfo.min_price_tip}} <i>|</i> {{poiInfo.shipping_fee_tip}} <i>|</i> {{poiInfo.delivery_time_tip}}
    </p>

    <p class="time">
      配送时间: {{poiInfo.shipping_time}}
    </p>

    <ul class="discounts" v-if="discounts_size > 0">
      <li v-for="item in poiInfo.discounts2">
        <img class="icon" :src="item.icon_url"/>
        <span class="text" v-text="item.info"></span>
      </li>
    </ul>
  </div>
</template>

<script>
    export default {
        name: "temp"
    }
</script>

<style scoped>

</style>
