<template>
  <div class="header">

    <!-- 顶部通栏 开始 -->
    <div class="top-wrapper">
      <div class="back-wrapper">
        <span class="icon-arrow_lift"></span>
      </div>

      <form class="search-wrapper">
        <span class="search-icon"></span>
        <input class="search-bar" type="text" placeholder="搜索店内商品"/>
      </form>

      <div class="more-wrapper">
        <a class="spelling-bt" href="#">拼单</a>
        <div class="more-bt">
          <i class="s-radius"></i>
          <i class="s-radius"></i>
          <i class="s-radius"></i>
        </div>
      </div>
    </div>
    <!-- 顶部通栏 结束 -->

    <!-- 中间内容部分 开始 -->
    <div class="content-wrapper">
      <div class="icon" :style="head_bg"></div>
      <div class="name">
        <span>{{poiInfo.name}}</span>
      </div>
      <div class="collect">
        <img src="./img/star.png"/>
        <span>收藏</span>
      </div>
    </div>
    <!-- 中间内容部分 结束 -->

    <!-- 活动提示部分 开始 -->
    <div class="bulletin-wrapper">
      <template v-if="discounts_size > 0">
        <img class="icon" :src="poiInfo.discounts2[0].icon_url" alt="icon"/>
        <span class="text" v-text="poiInfo.discounts2[0].info"></span>
      </template>
      <template v-else>
        <span>暂无活动</span>
      </template>
      <div class="detail" @click="showBulletin">
        {{discounts_size}}个活动
        <span class="icon-keyboard_arrow_right"></span>
      </div>
    </div>
    <!-- 活动提示部分 结束 -->

    <!-- 背景图片开始 -->
    <div class="bg-wrapper" :style="head_pic_url"></div>
    <!-- 背景图片结束 -->

    <!-- 公告详情 开始 -->
    <transition name="bulletin-detail">
      <div class="bulletin-detail" v-if="isBulletinShow">
        <div class="detail-wrapper">
          <div class="main-wrapper" :style="detail_bg">
            <div class="icon" :style="head_bg"></div>
            <h3 class="name">{{poiInfo.name}}</h3>

            <!-- 星级评价 -->
            <div class="score">
              <app-star :score="poiInfo.wm_poi_score"></app-star>
              <span>{{poiInfo.wm_poi_score}}</span>
            </div>

            <p class="tip">
              {{poiInfo.min_price_tip}} <i>|</i> {{poiInfo.shipping_fee_tip}} <i>|</i> {{poiInfo.delivery_time_tip}}
            </p>

            <p class="time">
              配送时间: {{poiInfo.shipping_time}}
            </p>

            <div class="discounts" v-if="discounts_size > 0">
              <p class="discounts-item" v-for="item in poiInfo.discounts2">
                <img class="icon" :src="item.icon_url" alt="icon"/>
                <span class="text" v-text="item.info"></span>
              </p>
            </div>
          </div>
          <!-- 关闭内容容器 -->
          <div class="close-wrapper">
            <span class="icon-close" @click="closeBulletin"></span>
          </div>
        </div>
      </div>
    </transition>
    <!-- 公告详情 结束 -->
  </div>
</template>

<script>
  import Star from '../star/Star'

  export default {
    name: "Header",
    components: {
      'app-star': Star
    },
    props: {
      poiInfo: {
        type: Object,
        default: {}
      }
    },
    data() {
      return {
        isBulletinShow: false
      }
    },
    computed: {
      head_pic_url() {
        return "background-image: url(" + this.poiInfo.head_pic_url + ")";
      },
      head_bg() {
        return "background-image: url(" + this.poiInfo.pic_url + ")";
      },
      detail_bg() {
        return "background-image: url(" + this.poiInfo.poi_back_pic_url + ")";
      },
      discounts_size() {
        return this.poiInfo.discounts2 ? this.poiInfo.discounts2.length : 0;
      }
    },
    methods: {
      showBulletin() {
        this.isBulletinShow = true
      },
      closeBulletin() {
        this.isBulletinShow = false
      }
    }
  }
</script>

<style scoped>

  @import url(../../common/css/icon.css);

  .header {
    height: 130px;
    padding-top: 20px;
  }

  /*背景图片样式*/
  .header .bg-wrapper {
    width: 100%;
    height: 150px;
    position: absolute;
    top: 0;
    left: 0;
    background-size: 100% 135%;
    background-position: center -12px;
    z-index: -1;
  }

  /* 顶部通栏样式 */
  .header .top-wrapper {
    position: relative;
  }

  .header .top-wrapper .back-wrapper {
    width: 50px;
    height: 31px;
    position: absolute;
    left: 0;
    top: 0;
    line-height: 31px;
    /*background-color: greenyellow;*/
    text-align: center;
  }

  .header .top-wrapper .back-wrapper span {
    display: inline-block;
    color: white;
  }

  .header .top-wrapper .search-wrapper {
    width: 100%;
    height: 31px;
    padding: 0 104px 0 50px;
    box-sizing: border-box;
  }

  .header .top-wrapper .search-wrapper .search-icon {
    width: 28px;
    height: 31px;
    background: url("./img/search.png") no-repeat 11px center;
    background-size: 13px 13px;
    position: absolute;
  }

  .header .top-wrapper .search-wrapper .search-bar {
    width: 100%;
    height: 31px;
    padding-left: 28px;
    border: 0;

    /* 设置盒子从边框开始计算*/
    box-sizing: border-box;
    background: #cdcdcc;
    border-radius: 25px;
    /* 去除选中时蓝色边框*/
    outline: none;
  }

  .header .top-wrapper .more-wrapper {
    width: 65px;
    height: 24px;
    /*background-color: pink;*/
    position: absolute;
    right: 0;
    top: 0;
    padding: 7px 15px 0 24px;
  }

  .header .top-wrapper .more-wrapper .spelling-bt {
    width: 30px;
    height: 17px;
    color: white;
    line-height: 17px;
    border: 1px solid white;
    text-align: center;
    float: left;
    text-decoration: none;
    font-size: 10px;
  }

  .header .top-wrapper .more-wrapper .more-bt {
    float: right;
    width: 20px;
    height: 24px;
    margin-left: 13px;
    margin-top: 6px;
  }

  .header .top-wrapper .more-wrapper .more-bt .s-radius {
    width: 3px;
    height: 3px;
    border-radius: 50%;
    border: 1px solid white;
    display: block;
    float: left;
    margin-right: 1px;
    z-index: -1;
  }

  /* 中间内容部分样式 */
  .header .content-wrapper {
    padding: 17px 10px 11px;
    height: 50px;
  }

  .header .content-wrapper .icon {
    width: 50px;
    height: 50px;
    background-size: 135% 100%;
    background-position: center;
    border-radius: 5px;
    float: left;
  }

  .header .content-wrapper .name {
    vertical-align: center;
    line-height: 50px;
    float: left;
    padding-left: 12px;
  }

  .header .content-wrapper .name span {
    color: white;
    font-size: 16px;
    font-weight: bold;
  }

  .header .content-wrapper .collect {
    width: 25px;
    height: 37px;
    float: right;
    text-align: center;
    padding-top: 6px;
  }

  .header .content-wrapper .collect img {
    width: 20px;
    height: 20px;
  }

  .header .content-wrapper .collect span {
    margin-top: 7px;
    color: white;
    font-size: 11px;
  }

  /* 公共栏样式 */
  .header .bulletin-wrapper {
    height: 16px;
    padding: 0 10px;
  }

  .header .bulletin-wrapper .icon {
    width: 16px;
    height: 16px;
    float: left;
    margin-right: 6px;
  }

  .header .bulletin-wrapper .text {
    font-size: 11px;
    color: white;
    float: left;
    line-height: 16px;
  }

  .header .bulletin-wrapper .detail {
    float: right;
    font-size: 11px;
    color: white;
    line-height: 16px;
  }

  .header .bulletin-wrapper .detail span {
    font-size: 16px;
    line-height: 16px;
    float: right;
  }

  /* 公告详情样式 */
  .header .bulletin-detail {
    position: absolute;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    background-color: rgba(98, 98, 98, 0.8);
    z-index: 999;
  }

  .header .bulletin-detail .detail-wrapper {
    width: 100%;
    height: 100%;
    padding: 43px 20px 125px;
    box-sizing: border-box;
  }

  .header .bulletin-detail .detail-wrapper .main-wrapper {
    width: 100%;
    height: 100%;
    background-size: 100% 100%;
    border-radius: 10px;
    text-align: center;
  }

  .header .bulletin-detail .detail-wrapper .main-wrapper .icon {
    width: 60px;
    height: 60px;
    background-size: 135% 100%;
    background-position: center;
    border-radius: 5px;
    display: inline-block;
    margin-top: 40px;
  }

  .header .bulletin-detail .detail-wrapper .main-wrapper .name {
    font-size: 15px;
    color: white;
    margin-top: 13px;
  }

  .header .bulletin-detail .detail-wrapper .main-wrapper .score {
    height: 10px;
    margin-top: 6px;
    text-align: center;
    font-size: 0;
  }

  .header .bulletin-detail .detail-wrapper .main-wrapper .score .star {
    display: inline-block;
    margin-right: 7px;
  }

  .header .bulletin-detail .detail-wrapper .main-wrapper .score span {
    display: inline-block;
    font-size: 10px;
    color: white;
  }

  .header .bulletin-detail .detail-wrapper .main-wrapper .tip {
    font-size: 11px;
    color: #bababc;
    margin-top: 8px;
  }

  .header .bulletin-detail .detail-wrapper .main-wrapper .tip i {
    margin: 0 7px;
  }


  .header .bulletin-detail .detail-wrapper .main-wrapper .time {
    font-size: 11px;
    color: #bababc;
    margin-top: 13px;
  }

  .header .bulletin-detail .detail-wrapper .main-wrapper .discounts {
    margin: 30px 20px;
    box-sizing: border-box;
    border-top: 1px solid #bababc;
    text-align: center;
    padding-top: 20px;
  }

  .header .bulletin-detail .detail-wrapper .main-wrapper .discounts .discounts-item {
    height: 16px;
    line-height: 16px;
    display: inline-block;
    margin-top: 8px;
    vertical-align: middle;
  }

  .header .bulletin-detail .detail-wrapper .main-wrapper .discounts .discounts-item .icon {
    width: 16px;
    height: 16px;
    line-height: 16px;
    margin: 0;
    float: left;
  }

  .header .bulletin-detail .detail-wrapper .main-wrapper .discounts .discounts-item .text {
    font-size: 11px;
    color: white;
    height: 16px;
    padding-left: 6px;
    line-height: 16px;
  }

  .header .bulletin-detail .detail-wrapper .close-wrapper {
    padding-top: 20px;
    height: 40px;
    text-align: center;
  }

  .header .bulletin-detail .detail-wrapper .close-wrapper span {
    width: 40px;
    height: 40px;
    line-height: 40px;
    border-radius: 50%;
    font-size: 14px;
    color: white;
    display: inline-block;
    background: rgba(118, 118, 118, 0.7);
    border: 1px solid rgba(140, 140, 140, 0.9);
  }

  .header
  .bulletin-detail-enter-active,
  .bulletin-detail-leave-active {
    transition: all 1s;
  }

  .header
  .bulletin-detail-enter,
  .bulletin-detail-leave-to {
    opacity: 0;
  }

  .header
  .bulletin-detail-enter-to,
  .bulletin-detail-leave {
    opacity: 1;
  }
</style>
