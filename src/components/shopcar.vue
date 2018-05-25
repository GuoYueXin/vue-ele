<template>
    <div class="wrap">
        <div class="content">
            <div class="chart-icon-wrap">
                <div class="icon-shopping_cart chart-icon" @click="changeFold" :class="{noCount : totalCount != 0}"></div>
                <div class="total-count" v-show="totalCount != 0">{{totalCount}}</div>
            </div>
            <div class="deliver-fee">
                <div class="price" :class="{hasPrice : totalPrice != 0}">￥{{totalPrice}}</div>
                <div class="deliverPrice">另需配送费￥{{this.deliveryPrice}}元</div>
            </div>
            <div class="deliver-base" :class="{ok : totalPrice >= this.minPrice}">{{diffPrice}}</div>
        </div>
        <div class="cart-list-wrap" v-show="this.fold">
          <div class="cart-gray" @click="changeFold"></div>
          <div class="cart-list">
            <div class="cart-list-title">
              <span class="cart-title">购物车</span><span class="clear" @click="clearChart()">清空</span>
            </div>
            <div class="food-item" v-for="food in selectFoods" :key="food.id">
              <span class="food-title">{{food.name}}</span>
              <cartcontral class="cart-contral" :food=food></cartcontral>
              <span class="price">{{food.price}}</span>
            </div>
          </div>
        </div>
    </div>
</template>
<script>
import cartcontral from './cartcontral'
export default {
    data(){
      return {
        fold : false
      }
    },
    props: {
        deliveryPrice:{
            type: Number
        },
        minPrice: {
            type : Number
        },
        selectFoods: {
          type: Array,
          default() {
            return [

            ]
          }
        }
    },
    components: {
      cartcontral
    },
    computed: {
        totalCount(){
            let totalCount = 0
            this.selectFoods.forEach((food) => {
              totalCount += food.count
            })
            return totalCount
        },
        showList(){
          if(!this.totalCount){
            this.fold = true;
            return false
          }
          return !this.fold;
          // return !this.showList
        },
        totalPrice(){
            let totalPrice = 0
            this.selectFoods.forEach((food) => {
              totalPrice += food.count * food.price
            })
            return totalPrice
        },
        diffPrice(){
          console.log(this.totalPrice)
            if(this.totalPrice === 0){
                return '还差￥' + this.minPrice + '元起送'
            }else if(this.totalPrice > 0 && this.totalPrice < this.minPrice){
                return '还差￥' + (this.minPrice - this.totalPrice) + '元起送'
            }else if(this.totalPrice >= this.minPrice){
                return '去结算'
            }
        }
    },
    methods: {
      changeFold(){
        if(this.totalCount){
          this.fold = !this.fold
        }
      },
      clearChart(){
        this.selectFoods.forEach(food => {
          food.count = 0
        })
        this.fold = false
      }
    }
}
</script>
<style lang='stylus' rel='stylesheet/stylus'>
@import '../../static/style.css'
.wrap
  position fixed
  width 100%
  height 48px
  left 0
  bottom 0
  z-index 300
  .content
    width 100%
    display flex
    background-color #141d27
    .chart-icon-wrap
      position relative
      flex 0 0 80px
      .chart-icon
        position relative
        width 44px
        height 44px
        margin-left 18px
        margin-top -10px
        border 6px solid #141d27
        border-radius 50%
        background-color #2b333b
        font-size 24px
        color rgba(255,255,255,0.4)
        line-height 44px
        text-align center
        &.noCount
            background-color #00a0dc
            color #ffffff
      .total-count
        position absolute
        top -6px
        right 0
        font-size 8px
        width 24px
        height 16px
        line-height 16px
        color #ffffff
        background-color red
        text-align center
        border-radius 16px
        font-weight 700
        box-shadow 0 4px 8px 0 rgba(0,0,0,0.4)
    .deliver-fee
        flex 1
        padding 12px 0
        height 24px
        .price
            display inline-block
            font-size 16px
            color rgba(255,255,255,0.4)
            font-weight 700
            line-height 24px
            padding-right 12px
            border-right 1px solid rgba(255,255,255,0.1)
            &.hasPrice
                color #ffffff
        .deliverPrice
            display inline-block
            padding-left 12px
            font-size 16px
            color rgba(255,255,255,0.4)
            font-weight 200
            line-height 24px
    .deliver-base
        flex 0 0 89px
        font-size 12px
        color rgba(255,255,255,0.4)
        font-weight 700
        line-height 48px
        background-color #2b333b
        text-align center
        padding 0 8px
        &.ok
            background-color green
            color #ffffff
  .cart-list-wrap
    position fixed
    left 0
    top 0
    bottom 48px
    display flex
    flex-direction  column
    z-index -1
    width 100%
    .cart-gray
      width 100%
      flex 1
      background-color rgba(7,17,27,0.6)
    .cart-list
      width 100%
      position fixed
      bottom 48px
      left 0
      background-color #ffffff
      overflow auto
      max-height 217px
      .cart-list-title
        height 40px
        line-height 40px
        background-color #f3f5f7
        padding 0 18px
        border-bottom 1px solid rgba(7,17,27,0.1)
        .cart-title
          font-size 14px
          font-weight 200
          color rgb(7,17,27)
          line-height 40px
          float left
        .clear
          font-size 12px
          line-height 40px
          color rgb(0,160,220)
          float right
      .food-item
        margin 0 18px
        padding 12px 0
        border-bottom 1px solid rgba(7,17,27,0.1)
        .food-title
          font-size 14px
          color rgb(7,17,27)
          line-height 24px
        .price
          float right
          font-size 14px
          font-weight 700
          color rgb(240,20,20)
          line-height 24px
          margin-right 12px
        .cart-contral
          float right
          line-height 24px





</style>
