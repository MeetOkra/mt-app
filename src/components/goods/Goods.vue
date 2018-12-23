<template>
  <div class="goods">
    <!--分类列表-->
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
        <!-- 专场 -->
        <li class="menu-item">
          <p class="text">
            <img class="icon" :src="container.tag_icon" v-if="container.tag_icon"/>
            {{container.tag_name}}
          </p>
        </li>

        <!-- 其他菜单 -->
        <li class="menu-item" v-for="(item, index) in goods" :key="index">
          <p class="text">
            <img class="icon" :src="item.icon" v-if="item.icon">
            {{item.name}}
          </p>
        </li>
      </ul>
    </div>
    <div class="goods-wrapper" ref="goodsWrapper"></div>
  </div>
</template>

<script>
  export default {
    name: "Goods",
    data() {
      return {
        container: {},
        goods: [],
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
    top: 190px;
    bottom: 51px;
    overflow: hidden;
    background-color: pink;
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

  .goods .menu-wrapper .menu-item .text .icon{
    width: 15px;
    height: 15px;
    vertical-align: middle;
  }

  .goods .goods-wrapper {
    flex: 1;
  }
</style>
