<template>
  <div class="goods">
    <!--分类列表-->
    <div class="menu-wrapper" ref="menuScroll">
      <ul>
        <!-- 专场 -->
        <li
          class="menu-item"
          :class="{'current': isCurrentMenuItem(0)}"
          @click="scrollToMenuItem(0)">
          <p class="text">
            <img class="icon" :src="container.tag_icon" v-if="container.tag_icon"/>
            {{container.tag_name}}
          </p>
        </li>

        <!-- 其他菜单 -->
        <li
          class="menu-item"
          v-for="(item, index) in goods"
          :key="index"
          :class="{'current': isCurrentMenuItem(index + 1)}"
          @click="scrollToMenuItem(index + 1)">
          <p class="text">
            <img class="icon" :src="item.icon" v-if="item.icon">
            {{item.name}}
          </p>
        </li>
      </ul>
    </div>
    <div class="goods-wrapper" ref="goodScroll">
      <!--专场-->
      <ul>
        <!-- 专场 -->
        <li class="container-list food-list-hook">
          <div v-for="(item,index) in container.operation_source_list" :key="index">
            <img :src="item.pic_url">
          </div>
        </li>
        <!-- 其他商品 -->
        <li v-for="(item, index) in goods" :key="index" class="goods-list food-list-hook">
          <h3 class="title">{{item.name}}</h3>
          <!-- 具体的商品列表 -->
          <ul>
            <li v-for="food in item.spus" :key="food.id" class="good-item">
              <div class="icon" :style="head_bg(food.picture)"></div>
              <div class="content">
                <h3 class="name">{{food.name}}</h3>
                <p class="desc" v-if="food.description">{{food.description}}</p>
                <div class="extra">
                  <span class="saled">{{food.month_saled_content}}</span>
                  <span class="praise">{{food.praise_content}}</span>
                </div>
                <img class="product" :src="food.product_label_picture" alt="">
                <p class="price">
                  <span class="text">${{food.min_price}}</span>
                  <span class="unit">/{{food.unit}}</span>
                </p>
              </div>
              <!--<div class="cartcontrol-wrapper">
                <app-cart-control :food="food"></app-cart-control>
              </div>-->
            </li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
  import BScroll from 'better-scroll';

  export default {
    name: "Goods",
    data() {
      return {
        container: {},
        goods: [],
        menuScroll: {},
        goodScroll: {},
        menuHeight: [],
        scrollY: 0,
        scrollDirection: 1 // 0- up，1-down
      }
    },
    methods: {
      head_bg(url) {
        return "background-image: url(" + url + ")";
      },
      initScroll() {
        this.menuScroll = new BScroll(this.$refs.menuScroll, {
          click: true
        });
        this.goodScroll = new BScroll(this.$refs.goodScroll, {
          probeType: 3,
          click: true
        })
      },
      calculateHeight() {
        let foodItems = this.$refs.goodScroll.getElementsByClassName("food-list-hook");
        let height = 0;
        let heights = [];
        heights.push(height);
        for (let foodItem of foodItems) {
          height += foodItem.clientHeight
          heights.push(height);
        }
        this.menuHeight = heights;
      },
      bindScrollEvent() {
        // 监听滚动的位置
        this.goodScroll.on("scroll", (pos) => {
          this.scrollY = pos.y;
        });
        // 根据滚动位置确认下标,与左侧对应
        // 通过下标实现点击左侧,滚动右侧
      },
      isCurrentMenuItem(index) {
        return this.findMenuItemIndexByScrollY(this.scrollY) === index;
      },
      findMenuItemIndexByScrollY(scrollY) {
        if (scrollY >= 0) {
          return 0
        } else {
          scrollY = -scrollY;
          let menuHeight = this.menuHeight;
          let length = menuHeight.length
          for (let index = 0; index < length - 1; index++) {
            let startHeight = menuHeight[index];
            let endHeight = menuHeight[index + 1];
            if (startHeight <= scrollY && scrollY < endHeight) {
              return index;
            }
          }
          return this.goods.length; // + 1(专场) - 1(下标)
        }
      },
      scrollToMenuItem(index) {
        this.scrollY = -this.menuHeight[index];
        this.goodScroll.scrollTo(0, this.scrollY);
      }
    },
    created() {
      fetch("/api/goods")
        .then(res => res.json())
        .then(response => {
          if (response.code === 0) {
            console.log(response.data)
            this.container = response.data.container_operation_source;
            this.goods = response.data.food_spu_tags;
            // DOM已经更新
            this.$nextTick(() => {
              // 执行滚动方法
              this.initScroll()

              // 计算分类的区间高度
              this.calculateHeight()

              //监听事件
              this.bindScrollEvent()
            })
          }
        })
    }
  }
</script>

<style scoped>
  .goods {
    display: flex;
    position: absolute;
    width: 100%;
    top: 191px;
    bottom: 51px;
    overflow: hidden;
  }

  /* 菜单样式 */
  .goods .menu-wrapper {
    flex: 0 0 85px;
    background: #f4f4f4;
  }

  .goods .menu-wrapper .menu-item {
    padding: 16px 23px 15px 10px;
    border-bottom: 1px solid #E4E4E4;
    position: relative;
  }

  /* 当前选中 */
  .goods .menu-wrapper .menu-item.current {
    background: white;
    font-weight: bold;
    margin-top: -1px;
  }

  .goods .menu-wrapper .menu-item:first-child.current {
    margin-top: 1px;
  }

  .goods .menu-wrapper .menu-item .text {
    font-size: 13px;
    color: #333333;
    line-height: 17px;
    vertical-align: middle;
    -webkit-line-clamp: 2;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }

  .goods .menu-wrapper .menu-item .text .icon {
    width: 15px;
    height: 15px;
    vertical-align: middle;
  }

  /* 商品列表 */
  .goods .goods-wrapper {
    flex: 1;
  }

  .goods .goods-wrapper .container-list {
    padding: 11px 11px 0 11px;
    border-bottom: 1px solid #E4E4E4;
  }

  .goods .goods-wrapper .container-list img {
    width: 100%;
    margin-bottom: 11px;
    border-radius: 5px;
  }

  .goods .goods-wrapper .goods-list {
    padding: 11px;
  }

  .goods .goods-wrapper .goods-list .title {
    height: 13px;
    font-size: 13px;
    background: url(./img/btn_yellow_highlighted@2x.png) no-repeat left center;
    background-size: 2px 10px;
    padding-left: 7px;
    margin-bottom: 12px;
    color: #000;
  }

  .goods .goods-wrapper .goods-list .good-item {
    display: flex;
    margin-bottom: 25px;
    position: relative;
  }

  .goods .goods-wrapper .goods-list .good-item .icon {
    flex: 0 0 63px;
    background-position: center;
    background-size: 120% 100%;
    background-repeat: no-repeat;
    margin-right: 11px;
    height: 75px;
  }

  .goods .goods-wrapper .goods-list .good-item .content {
    flex: 1;
  }

  .goods .goods-wrapper .goods-list .good-item .content .name {
    font-size: 16px;
    line-height: 21px;
    color: #333333;
    margin-bottom: 10px;
    padding-right: 27px;
  }

  .goods .goods-wrapper .goods-list .good-item .content .desc {
    font-size: 10px;
    line-height: 19px;
    color: #bfbfbf;
    margin-bottom: 8px;

    /* 超出部分显示省略号*/
    -webkit-line-clamp: 1;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }

  .goods .goods-wrapper .goods-list .good-item .content .extra {
    font-size: 10px;
    color: #BFBFBF;
    margin-bottom: 7px;
  }

  .goods .goods-wrapper .goods-list .good-item .content .extra .saled {
    margin-right: 14px;
  }

  .goods .goods-wrapper .goods-list .good-item .content .product {
    height: 15px;
    margin-bottom: 6px;
  }

  .goods .goods-wrapper .goods-list .good-item .content .price {
    font-size: 0;
  }

  .goods .goods-wrapper .goods-list .good-item .content .price .text {
    font-size: 14px;
    color: #fb4e44;
  }

  .goods .goods-wrapper .goods-list .good-item .content .price .unit {
    font-size: 12px;
    color: #BFBFBF;
  }
</style>
