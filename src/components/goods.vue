<template>
  <div class="goods">
      <div class="menu-wrap" ref="menuWrapper">
        <div class="menu-item" v-for="(item,index) in goods" :key="index" :class="{current : currentIndex === index}" @click="selectIndex(index,$event)">
          <div class="text-wrap">
            <span :class="classMap[item.type]" v-show="item.type > 0" class="icon"></span>
            <span class="text">{{item.name}}</span>
          </div>
        </div>
      </div>
      <div class="foods-wrap" ref="foodWrapper">
        <ul>
          <li class="food-list-hook" v-for="(item1, index) in goods" :key="index">
            <div class="item-title">{{item1.name}}</div>
            <ul>
              <li class="food-details" v-for="(item2, index) in item1.foods" :key="index">
                <div class="food-icon">
                  <img :src="item2.icon">
                </div>
                <div class="food-content">
                  <div class="food-title">{{item2.name}}</div>
                  <div class="food-desc" v-show="item2.description">{{item2.description}}</div>
                  <div class="food-sell-rating">
                    <span class="sell-count">月售{{item2.sellCount}}份</span>
                    <span class="rating" style="margin-left:12px">{{item2.rating}}%</span>
                  </div>
                  <div class="food-price">
                    <span class="new-price"><span>￥</span>{{item2.price}}</span>
                    <span class="old-price" v-show="item2.oldPrice" style="margin-left:8px"><span>￥</span>{{item2.oldPrice}}</span>
                    <div class="cart-contral-wrap">
                      <cartcontral :food=item2></cartcontral>
                    </div>
                  </div>
                </div>
              </li>
            </ul>
          </li>
        </ul>
      </div>
      <shopcar :selectFoods=selectFoods :minPrice="seller.minPrice" :deliveryPrice="seller.deliveryPrice"></shopcar>
  </div>
</template>
<script>
import axios from 'axios'
import vue from 'vue'
import BScroll from 'better-scroll'
import shopcar from './shopcar'
import cartcontral from './cartcontral'
export default {
  name: 'goods',
  props: {
    seller: {
      type : Object,
      default: {}
    }
  },
  data() {
    return {
      goods : [],
      classMap : ['decrease','discount','special','invoice_1','guarantee'],
      heightList : [],
      scrollY : 0
    }
  },
  created(){
    axios.get('/good/goods').then(res =>{
      if(res.data.code === 0){
        this.goods = res.data.data
        vue.nextTick(() => {
          this._initScroll(),
          this._caculateHeight()
        })
      }
    })
  },
  components : {
    shopcar,
    cartcontral,
  },
  computed: {
    currentIndex(){
      for(let i = 0; i < this.heightList.length; i++){
        let height1 = this.heightList[i]
        let height2 = this.heightList[i + 1]
        if( !height2 || (this.scrollY >= height1 && this.scrollY < height2)){
          return i
        }

      }
      return 0
    },
    selectFoods(){
      let foods= [];
      if(this.goods.length !== 0){
        this.goods.forEach(good => {
          good.foods.forEach(food => {
            if(food.count){
              foods.push(food)
            }
          })
        })
      }
      return foods
    }
  },
  methods: {
    selectIndex($index, $event){
      if(!$event._constructed){
        return
      }
      let foodList = this.$refs.foodWrapper.getElementsByClassName('food-list-hook')

      this.foodScroll.scrollToElement(foodList[$index],300)
    },
    _initScroll(){
      this.menuScroll = new BScroll(this.$refs.menuWrapper,{
        click: true
      })
      this.foodScroll = new BScroll(this.$refs.foodWrapper,{
        probeType: 3,
        click: true
      })
      this.foodScroll.on('scroll',(pos) => {
        this.scrollY = Math.abs(Math.round(pos.y))
      })
    },
    _caculateHeight(){
      let foodList = this.$refs.foodWrapper.getElementsByClassName('food-list-hook')
      let height = 0
      this.heightList.push(height)
      for(let i = 0, len = foodList.length; i < len; i++){
        let item = foodList[i]
        height += item.clientHeight
        this.heightList.push(height)
      }
    }
  }
}
</script>
<style lang='stylus' rel='stylesheet/stylus'>
.goods
  position absolute
  display flex
  top 177px
  bottom 48px
  left 0
  right 0
  width 100%
  overflow hidden
  .icon
    display inline-block
    width 14px
    height 14px
    background-size 14px 14px
    vertical-align top
    &.decrease
      background-image url('../assets/img/decrease_3@2x.png')
    &.discount
      background-image url('../assets/img/discount_2@2x.png')
    &.special
      background-image url('../assets/img/special_3@2x.png')
    &.invoice_1
      background-image url('../assets/img/invoice_3@2x.png')
    &.guarantee
      background-image url('../assets/img/guarantee_3@2x.png')
  .menu-wrap
    flex 0 0 80px
    width 80px
    background #f3f5f7
    overflow auto
    .menu-item
      padding 0 12px
      font-size 0
      line-height 16px
      &.current
        background-color #ffffff
        margin-top -1px
        position relative
        &:after
          border-top 1px solid #ffffff
        .text-wrap
          .text
            font-weight 500
            font-size 12px
            line-height 14px
      .text-wrap
        display table-cell
        vertical-align middle
        width 56px
        height 54px
        border-bottom 1px solid rgba(7,17,27,0.1)

        .text
          font-size 12px
          line-height 14px
          font-weight 200
  .foods-wrap
    flex 1
    background #ffffff
    overflow auto
    .item-title
      height 26px
      font-size 12px
      line-height 26px
      color rgb(147,153,159)
      background-color #f3f5f7
      border-left 4px solid #d9dde1
      padding-left 14px
    .food-details
      position relative
      padding 18px
      border-bottom 1px solid rgba(7,17,27,0.1)
      &:last-child
        border-bottom none
      .food-icon
        width 58px
        height 58px
        float left
        img
          width 58px
          height 58px
      .food-content
        position relative
        margin-left 58px
        padding-left 10px
        padding-top 2px
        .food-title
          font-size 14px
          color rgb(7,17,27)
          line-height 14px
        .food-desc,
        .food-sell-rating
          margin-top 8px
          font-size 10px
          color rgb(147,153,159)
          line-height 12px
        .food-price
          margin-top 8px
          .new-price
            font-size 14px
            color rgb(240,20,20)
            font-weight 700
            line-height 20px
            span
              font-size 10px
              font-weight normal
          .old-price
            vertical-align top
            font-size 10px
            color rgb(147,153,159)
            line-height 24px
            font-weight 700
            text-decoration line-through
            span
              font-weight normal
          .cart-contral-wrap
            position absolute
            right 0
            bottom 0



</style>
