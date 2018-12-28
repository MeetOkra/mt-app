<template>
  <div class="comment">
    <div class="comment-left">
      <img class="head-icon" :src="headIconUrl">
    </div>
    <div class="comment-right">
      <div class="comment-info">
        <span class="name">{{comment.user_name}}</span>
        <span class="time">{{comment.comment_time}}</span>
      </div>
      <p class="comment_content">{{comment.comment_content}}</p>
    </div>
  </div>
</template>

<script>
  export default {
    name: "Comment",
    props: {
      comment: {
        type: Object,
        required: true
      }
    },
    computed: {
      headIconUrl() {
        if (this.comment.user_icon) {
          return this.comment.user_icon
        }
        return './img/anonymity.png'
      }
    }
  }
</script>

<style scoped>
  .comment {
    width: 100%;
    height: auto;
    position: relative;
    padding-bottom: 10px;
    border-bottom: 1px solid #F4F4F4;
  }

  .comment .comment-left {
    width: 60px;
    text-align: center;
    float: left;
  }

  .comment .comment-left .head-icon {
    width: 50px;
    height: 50px;
    border: 1px solid #e4e4e4;
    border-radius: 50%;
    background-size: 100% 100%;
  }

  .comment .comment-right {
    margin-left: 55px;
    padding: 0 10px;
    box-sizing: border-box;
  }

  .comment .comment-right .comment-info {
    line-height: 25px;
  }

  .comment .comment-right .comment-info .name {
    font-size: 12px;
    color: #333333;
  }

  .comment .comment-right .comment-info .time {
    font-size: 13px;
    color: #666666;
    float: right;
  }
  .comment .comment-right .comment_content {
    font-size: 11px;
    padding: 5px 0;
  }
</style>
