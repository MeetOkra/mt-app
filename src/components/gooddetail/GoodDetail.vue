<template>
  <transition name="detail-anim">
    <div class="detail" v-show="isShow" ref="detail">
      <div>
        <div class="top-wrapper">
          <span class="icon-close top-left-icon" @click="hidePage"></span>
          <div class="top-right">
            <img src="./img/share.png">
            <img src="./img/more.png">
          </div>
        </div>
        <div class="product-images">
          <img :src="good.picture">
        </div>
        <div class="product-info">
          <h3 class="name">{{good.name}}</h3>
          <p class="sell-num">{{good.month_saled_content}}</p>
          <img class="picture" :src="good.product_label_picture" v-if="good.product_label_picture">
          <div class="product-info-bottom">
            <div class="bottom-left">
              <span class="price">¥ {{good.min_price}}</span><span class="unit" v-if="good.unit">/{{good.unit}}</span>
            </div>
            <div class="bottom-right">
              <div class="cart-control-wrapper">
                <cart-control :good="good"></cart-control>
              </div>
              <span class="choose">选规格</span>
            </div>
          </div>
        </div>
        <split></split>
        <div class="rating">
          <div class="rating-header">
            <span class="title">外卖评价</span>
            <div class="rating-num">
              {{calcCommentCount}}条评论
              <span class="icon-keyboard_arrow_right"></span>
            </div>
          </div>
          <ul class="rating-body">
            <li v-for="(comment, index) in fetchCommentList" :key="index" class="comment-item">
              <app-comment :comment="comment"></app-comment>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
  import BScroll from 'better-scroll'
  import Split from '../split/Split'
  import Comment from '../comment/Comment'
  import CartControl from '../cardcontrol/CardControl'

  export default {
    name: "GoodDetail",
    data() {
      return {
        isShow: false,
        scroll: undefined
      }
    },
    props: {
      good: {
        type: Object,
        required: true
      }
    },
    methods: {
      showPage() {
        this.isShow = true
        if (!this.scroll) {
          this.scroll = new BScroll(this.$refs.detail, {
            click: true
          })
        } else {
          this.scroll.refresh()
        }
      },
      hidePage() {
        this.isShow = false
      }
    },
    computed: {
      calcCommentCount() {
        if (this.good && this.good.rating && this.good.rating.comment_count) {
          return this.good.rating.comment_count
        }
        return 0
      },
      fetchCommentList() {
        if (this.good && this.good.rating && this.good.rating.comment_list) {
          return this.good.rating.comment_list
        }
        return []
      }
    },
    components: {
      "split": Split,
      "app-comment": Comment,
      "cart-control": CartControl
    },
  }
</script>

<style scoped>
  @import "../../common/css/icon.css";

  .detail-anim-enter-active, .detail-anim-leave-active {
    transition: all .5s;
  }

  .detail-anim-leave-to, .detail-anim-enter {
    transform: translateX(100%);
  }

  .detail {
    right: 0;
    top: 0;
    left: 0;
    bottom: 51px;
    position: fixed;
    background-color: white;
  }

  .detail .top-wrapper {
    width: 100%;
    height: 50px;
    line-height: 50px;
    padding: 0 10px;
    box-sizing: border-box;
  }

  .detail .top-wrapper .top-left-icon {
    width: 30px;
    height: 30px;
    background-color: #7f7f7f;
    float: left;
    color: white;
    font-size: 30px;
    border-radius: 50%;
    line-height: 30px;
    text-align: center;
    position: relative;
    top: 50%;
    transform: translateY(-50%);
  }

  .detail .top-right {
    float: right;
    height: 30px;
    position: relative;
    top: 50%;
    transform: translateY(-50%);
  }

  .detail .top-right img {
    width: 30px;
    height: 30px;
  }

  .detail .product-images {
    position: relative;
    width: 100%;
    height: 0;
    padding-top: 100%;
  }

  .detail .product-images img {
    position: absolute;
    left: 0;
    bottom: 0;
    width: 100%;
    height: 100%;
  }

  .detail .product-info {
    padding: 0 0 16px 16px;
    position: relative;
  }

  .detail .product-info .name {
    font-size: 15px;
    color: #333333;
    line-height: 30px;
    font-weight: bold;
  }

  .detail .product-info .sell-num {
    font-size: 11px;
    color: #9d9d9d;
    margin-bottom: 6px;
  }

  .detail .product-info .picture {
    height: 15px;
    margin-bottom: 16px;
  }

  .detail .product-info .product-info-bottom {
  }

  .detail .product-info .product-info-bottom .bottom-left {
    font-size: 0;
  }

  .detail .product-info .product-info-bottom .price {
    font-size: 17px;
    color: #FB4E44;
  }

  .detail .product-info .product-info-bottom .unit {
    font-size: 11px;
    color: #9D9D9D;
  }

  .detail .product-info .product-info-bottom .bottom-right {
    position: absolute;
    right: 12px;
    bottom: 10px;
    padding: 2px;
  }

  .detail .product-info .product-info-bottom .bottom-right .cart-control-wrapper {
    position: absolute;
    right: 12px;
    bottom: 10px;
    padding: 2px;
  }

  .detail .product-info .product-info-bottom .bottom-right .choose {
    width: 64px;
    height: 30px;
    font-size: 12px;
    line-height: 30px;
    text-align: center;
    background: #FFD161;
    border-radius: 30px;
    position: absolute;
    right: 12px;
    bottom: 10px;
  }

  .detail .rating {
    height: 100%;
    width: 100%;
  }

  .detail .rating .rating-header {
    height: 30px;
    padding: 10px;
  }

  .detail .rating .rating-header .title {
    font-size: 13px;
    color: #9D9D9D;
    float: left;
  }

  .detail .rating .rating-header .rating-num {
    float: right;
    font-size: 11px;
    color: #9D9D9D;
  }

  .detail .rating .rating-body {

  }

  .detail .rating .rating-body .comment-item {
    height: 100px;
  }
</style>
