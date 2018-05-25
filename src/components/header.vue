<template>
  <div class="header">
    <div class="content-wrap">
      <div class="avatar">
        <img :src="seller.avatar" alt="" class="avatar-img">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand bg-img"></span>
          <span>{{seller.name}}</span>
        </div>
        <div class="desc">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div v-if="seller.supports" class="reduce-price">
          <span class="reduce bg-img" :class="classMap[seller.supports[0].type]"></span>
          <span class="reduce-content">{{seller.supports[0].description}}</span>
        </div>
        <div v-if="seller.supports" class="num-wrap">
          <span class="num">{{seller.supports.length}}个</span>
          <span class="icon-keyboard_arrow_right"></span>
        </div>
      </div>
    </div>
    <div class="notice">
      <span class="notice-icon"></span>
      <span class="content">{{seller.bulletin}}</span>
      <span class="icon-keyboard_arrow_right"></span>
    </div>
    <div class="background-img">
      <img :src="seller.avatar" alt="">
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'header',
  data() {
    return {

    }
  },
  props :{
    seller : {
      type : Object,
      default : { score : 0.0},
      required : true
    },
    mz : {
      type : String
    }
  },
  created() {
    
  },
  watch: {
    seller: function () {
      this.classMap = ['decrease','discount','special','invoice_1','guarantee']
    }
  }
}
</script>
<style lang='stylus' rel='stylesheet/stylus' scoped>
  @import url('../../static/style.css');
  .header
    overflow hidden
    position relative
    .content-wrap
      position relative
      overflow hidden
      padding 24px 24px 18px
      background rgba(7,17,27,0.5)
      .avatar
        float left
      img 
        width 64px
        height 64px
        border-radius 2px 
      .content
        margin-left 64px
        padding-left 16px
        color rgb(255,255,255)
        .title
          padding-top 2px
          .brand
            background-image url('../assets/img/brand@2x.png')
            background-repeat no-repeat
            background-size 30px 18px
            width 30px
            height 18px
            display inline-block
            vertical-align top
          span 
            line-height 18px
            font-size 16px
            font-weight bold
        .desc
          margin-top 8px
          font-size 12px
          font-weight 200
          line-height 12px 
        .reduce-price
          margin-top 10px
          .reduce
            display inline-block
            // background-image url('../assets/img/decrease_2@2x.png')
            background-size 16px 16px
            width 16px
            height 16px
            vertical-align top
            &.decrease
              background-image url('../assets/img/decrease_2@2x.png')
            &.discount
              background-image url('../assets/img/discount_2@2x.png')
            &.special
              background-image url('../assets/img/special_2@2x.png')
            &.invoice_1
              background-image url('../assets/img/invoice_1@2x.png')
            &.guarantee
              background-image url('../assets/img/guarantee_2@2x.png')
          .reduce-content
            display inline-block
            padding-left 4px
            font-size 10px
            font-weight 200
            line-height 12px
        .num-wrap
          position absolute 
          right 12px
          bottom 14px
          color rgb(255,255,255)
          line-height 12px
          padding 7px 8px
          background-color rgba(0,0,0,0.2)
          border-radius 12px
          .num
            font-size 10px;
            font-weight 200
            padding-right 2px
          .icon-keyboard_arrow_right
            vertical-align middle
    .notice
      position relative
      height 28px
      line-height 28px
      background rgba(7,17,27,0.2)
      white-space nowrap
      text-overflow ellipsis
      overflow hidden
      padding 0 16px 0 12px
      font-size 10px
      font-weight 200
      color rgb(255,255,255)
      .notice-icon
        display inline-block
        width 22px
        height 12px
        background-image url('../assets/img/bulletin@2x.png')
        background-size 22px 12px
        vertical-align middle
        line-height 28px
      .content
        margin-left 4px
        margin-right 4px
        vertical-align middle
      .icon-keyboard_arrow_right
        position absolute 
        right 12px
        bottom 8px
        vertical-align middle
    .background-img
      position absolute 
      left 0
      top 0
      z-index -1
      filter blur(10px)
      width 100%
      overflow hidden
      img
        background-size cover
        width 100%
</style>
