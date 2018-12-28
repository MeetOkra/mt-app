<template>
  <div class="split-line" :style="{height: calcHeight, backgroundColor: bgColor}"></div>
</template>

<script>
  export default {
    name: "Split",
    props: {
      height: {
        type: Number,
        default: 10
      },
      bgColor: {
        type: String,
        default: "#e4e4e4"
      }
    },
    computed: {
      calcHeight() {
        return this.height + "px";
      }
    }
  }
</script>

<style scoped>
  .split-line {
    width: 100%;
  }
</style>
