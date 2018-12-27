<template>
  <div>
    <div class="shop-cart-wrapper">
      <div class="shopping-cart-left">
        <div class="cart-icon-wrapper" :class="{'high-light': this.calcGoodsCount}">
        <span
          class="icon-shopping_cart shopping-cart"
          @click="toggleDetailVisible"
        ></span>
          <p class="cart-tip" v-show="calcGoodsCount">{{calcGoodsCount}}</p>
        </div>
        <div class="cart-cost-wrapper">
          <p
            class="total-cost"
            v-show="calcGoodsCost > 0"
          >
            ¥ {{calcGoodsCost}}
          </p>
          <p
            class="shipping-fee"
            :class="{'high-light': this.calcGoodsCount}"
          >
            另需{{this.poiInfo.shipping_fee_tip}}
          </p>
        </div>
      </div>
      <div
        class="shopping-cart-right"
        :class="{'high-light': this.calcGoodsCount}"
      >
        <span>{{payStr}}</span>
      </div>

      <div class="cart-detail-wrapper" @click.stop :class="{'show': isShowDetail}" v-show="isShowDetail">
        <div class="discounts">
          <span>{{ discountInfo }}</span>
        </div>

        <div class="title">
          <h3 class="title-nick">1号口袋</h3>
          <div class="title-right" @click="clearShopCart">
            <img class="clear" src="./img/ash_bin.png">
            <span class="clear-text">清空购物车</span>
          </div>
        </div>

        <div class="goods-list" ref="goodsList">
          <ul>
            <li class="good-item" v-for="(good, index) in this.selectGoods" :key="index">
              <div class="good-item-left">
                <div class="good-item-detail">
                  <p class="good-name">{{good.name}}</p>
                  <p class="good-unit" v-if="good.unit">{{good.unit}}</p>
                  <p class="good-description" v-if="!good.unit">{{good.description}}</p>
                </div>
                <div class="good-item-price">
                  <p class="good-price">¥ {{calcGoodCount(good)}}</p>
                </div>
              </div>
              <div class="good-item-right">
                <cart-control :good="good"></cart-control>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </div>
    <div class="shop-cart-mask" v-show="isShowDetail" @click="closeCartDetail"></div>
  </div>
</template>

<script>
  import BScroll from 'better-scroll'
  import CardControl from '../cardcontrol/CardControl'

  export default {
    name: "ShopCart",
    data() {
      return {
        //是否折叠起来
        isFold: true,
        scroll: undefined
      }
    },
    props: {
      selectGoods: {
        type: Array,
        default: []
      },
      poiInfo: {
        type: Object,
        default: {}
      }
    },
    components: {
      "cart-control": CardControl
    },
    methods: {
      toggleDetailVisible() {
        if (this.calcGoodsCount) {
          this.isFold = !this.isFold
        }
      },
      calcGoodCount(good) {
        if (good.count <= 0) {
          return 0
        }
        return Math.round(good.count * good.min_price * 100) / 100
      },
      closeCartDetail() {
        this.isFold = true
      },
      clearShopCart() {
        this.selectGoods.forEach(good => {
          good.count = 0
        })
      }
    },
    computed: {
      discountInfo() {
        if (this.poiInfo.discounts2 &&
          this.poiInfo.discounts2.length > 0) {
          return this.poiInfo.discounts2[0].info
        }
        return ""
      },
      isShowDetail() {
        if (this.calcGoodsCount <= 0) {
          this.isFold = true
        }
        if (!this.isFold) {
          this.$nextTick(() => {
            console.log(this.scroll)
            if (!this.scroll) {
              this.scroll = new BScroll(this.$refs.goodsList, {
                click: true
              })
            } else {
              this.scroll.refresh()
            }
          })
        }
        return !this.isFold
      },
      calcGoodsCount() {
        let count = 0;
        this.selectGoods.forEach(good => {
          count += good.count;
        })
        return count
      },
      calcGoodsCost() {
        let cost = 0;
        this.selectGoods.forEach(good => {
          cost += Math.round(good.min_price * good.count * 100) / 100;
        });
        return cost
      },
      payStr() {
        return this.calcGoodsCount > 0 ? "去结算" : this.poiInfo.min_price_tip
      }
    }
  }
</script>

<style scoped>
  @import "../../common/css/icon.css";

  .shop-cart-wrapper {
    width: 100%;
    height: 51px;
    background: #514f4f;
    position: fixed;
    left: 0;
    bottom: 0;
    display: flex;
    z-index: 99
  }

  .shop-cart-wrapper .shopping-cart-left {
    flex: 1;
  }

  .shop-cart-wrapper .shopping-cart-left .cart-icon-wrapper {
    width: 50px;
    height: 50px;
    background: #666666;
    border-radius: 50%;
    position: relative;
    top: -14px;
    left: 10px;
    text-align: center;
    float: left;
  }

  .shop-cart-wrapper .shopping-cart-left .cart-icon-wrapper.high-light {
    background-color: #FFD161;
  }

  .shop-cart-wrapper .shopping-cart-left .cart-icon-wrapper .cart-tip {
    width: 15px;
    height: 15px;
    line-height: 15px;
    border-radius: 50%;
    font-size: 9px;
    color: white;
    background: red;
    position: absolute;
    right: 0;
    top: 0;
  }

  .shop-cart-wrapper .shopping-cart-left .cart-icon-wrapper .shopping-cart {
    font-size: 28px;
    color: #c4c4c4;
    line-height: 50px;
  }

  .shop-cart-wrapper .shopping-cart-left .cart-icon-wrapper.high-light .shopping-cart {
    color: #2D2B2A;
  }

  .shop-cart-wrapper .shopping-cart-left .cart-cost-wrapper {
    float: left;
    margin-left: 18px;
  }

  .shop-cart-wrapper .shopping-cart-left .cart-cost-wrapper .total-cost {
    font-size: 18px;
    line-height: 33px;
    color: white;
  }

  .shop-cart-wrapper .shopping-cart-left .cart-cost-wrapper .shipping-fee {
    font-size: 12px;
    color: #bab9b9;
    line-height: 51px;
  }

  .shop-cart-wrapper .shopping-cart-left .cart-cost-wrapper .shipping-fee.high-light {
    line-height: 12px;
  }

  .shop-cart-wrapper .shopping-cart-right {
    flex: 0 0 110px;
    font-size: 15px;
    color: #BAB9B9;
    line-height: 51px;
    text-align: center;
    font-weight: bold;
  }

  .shop-cart-wrapper .shopping-cart-right.high-light {
    background-color: #FFD161;
    color: #2D2B2A;
  }

  .shop-cart-wrapper .cart-detail-wrapper {
    position: absolute;
    left: 0;
    top: 0;
    z-index: -1;
    width: 100%;
  }

  .shop-cart-wrapper .cart-detail-wrapper.show {
    transform: translateY(-100%);
  }

  .shop-cart-wrapper .cart-detail-wrapper .discounts {
    height: 30px;
    text-align: center;
    font-size: 11px;
    background: #f3e6c6;
    line-height: 30px;
    color: #646158;
  }

  .shop-cart-wrapper .cart-detail-wrapper .title {
    height: 30px;
    background: #F4F4F4;
  }

  .shop-cart-wrapper .cart-detail-wrapper .title .title-nick {
    float: left;
    border-left: 4px solid #53c123;
    padding-left: 6px;
    line-height: 30px;
    font-size: 12px;
  }

  .shop-cart-wrapper .cart-detail-wrapper .title .title-right {
    float: right;
    line-height: 30px;
    margin-right: 10px;
    font-size: 0;
  }

  .shop-cart-wrapper .cart-detail-wrapper .title .title-right .clear {
    height: 14px;
    margin-right: 9px;
    vertical-align: middle;
  }

  .shop-cart-wrapper .cart-detail-wrapper .title .title-right .clear-text {
    font-size: 12px;
    vertical-align: middle;
  }

  .shop-cart-wrapper .cart-detail-wrapper .goods-list {
    max-height: 360px;
    overflow: hidden;
    background: white;
  }

  .shop-cart-wrapper .cart-detail-wrapper .goods-list .good-item {
    height: 38px;
    padding: 12px 12px 10px 12px;
    border-bottom: 1px solid #F4F4F4;
  }

  .shop-cart-wrapper .cart-detail-wrapper .goods-list .good-item .good-item-left {
    float: left;
    width: 240px;
  }

  .shop-cart-wrapper .cart-detail-wrapper .goods-list .good-item .good-item-right {
    float: right;
    width: 80px;
    text-align: right;
  }

  .shop-cart-wrapper .cart-detail-wrapper .goods-list .good-item .good-item-left .good-item-detail {
    float: left;
    width: 170px;

  }

  .shop-cart-wrapper .cart-detail-wrapper .goods-list .good-item .good-item-left .good-item-detail .good-name {
    font-size: 16px;
    margin-bottom: 8px;

    /* 超出部分隐藏*/
    -webkit-line-clamp: 1;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    overflow: hidden;
    height: 16px;
  }

  .shop-cart-wrapper .cart-detail-wrapper .goods-list .good-item .good-item-left .good-item-detail .good-unit {
    font-size: 12px;
    color: #B4B4B4;
  }

  .shop-cart-wrapper .cart-detail-wrapper .goods-list .good-item .good-item-left .good-item-detail .good-description {
    font-size: 12px;
    color: #B4B4B4;

    /* 超出部分隐藏*/
    overflow: hidden;
    height: 12px;
  }

  .shop-cart-wrapper .cart-detail-wrapper .goods-list .good-item .good-item-left .good-item-price {
    font-size: 12px;
    line-height: 38px;
  }

  .shop-cart-wrapper .cart-detail-wrapper .goods-list .good-item .good-item-right {
    float: right;
    margin-top: 6px;
  }

  .shop-cart-mask {
    position: fixed;
    top: 0;
    right: 0;
    width: 100%;
    height: 100%;
    z-index: 98;
    background: rgba(7, 17, 27, 0.6);
  }
</style>
