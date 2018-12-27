<template>
  <div class="control-wrapper">
    <transition name="move">
      <div
        class="icon-remove_circle_outline cart-decrease"
        @click="decreaseCartCount"
        v-show="this.good && this.good.count > 0"
      >
      </div>
    </transition>
    <div class="count" v-show="this.good && this.good.count > 0">
      {{ this.good.count }}
    </div>
    <div
      class="icon-add_circle cart-increase"
      @click="increaseCartCount">
      <i class="bg"></i>
    </div>
  </div>
</template>

<script>
  import Vue from 'vue'

  export default {
    name: "CardControl",
    props: {
      good: {
        type: Object,
        required: true
      }
    },
    methods: {
      decreaseCartCount() {
        if (this.good.count > 0) {
          this.good.count--
        }
      },
      increaseCartCount() {
        if (!this.good) {
          return
        }
        if (this.good && !this.good.count) {
          Vue.set(this.good, 'count', 1)
        } else {
          this.good.count++
        }
      }
    }
  }
</script>

<style scoped>

  @import "../../common/css/icon.css";

  .control-wrapper {
    font-size: 0;
  }

  .control-wrapper .cart-decrease {
    width: 26px;
    height: 26px;
    font-size: 26px;
    color: #b4b4b4;
    display: inline-block;
  }

  .control-wrapper .cart-increase {
    width: 26px;
    height: 26px;
    font-size: 26px;
    color: #ffd161;
    display: inline-block;
    position: relative;
  }

  .control-wrapper .cart-increase .bg {
    width: 20px;
    height: 20px;
    border-radius: 50%;
    background: black;
    position: absolute;
    left: 3px;
    top: 3px;
    z-index: -1;
  }

  .control-wrapper .count {
    display: inline-block;
    width: 25px;
    text-align: center;
    font-size: 12px;
    line-height: 26px;
    vertical-align: top;
  }

  .control-wrapper .move-enter-active,.move-leave-active {
    transition: all .5s linear;
  }
  .control-wrapper .move-enter, .move-leave-to {
    transform: translateX(20px) rotate(180deg)
  }

</style>
