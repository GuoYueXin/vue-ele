<template>
  <div class="cart-control">
    <span class="desrease icon-remove_circle_outline" v-show="food.count > 0" @click="del"></span>
    <span class="count" v-show="food.count > 0">{{food.count}}</span>
    <span class="increase icon-add_circle" @click="add"></span>
  </div>
</template>

<script>
import Vue from "vue";
export default {
  props: {
    food: {
      type: Object
    }
  },
  methods: {
    del(event) {
      if(event._constructed){
        if(this.food.count === 0){
          return
        }
        this.food.count --
      }
    },
    add(event) {
      if(event._constructed){
        if(!this.food.count){
          Vue.set(this.food,'count')
          this.food.count = 1
        }else{
          this.food.count ++
        }
        console.log(this.food.count)
      }
    }
  }
}
</script>
<style lang="stylus" rel="stylesheet/stylus" scoped>
  .cart-control
    font-size 10px
    height 24px
    .desrease,
    .increase
      color #0093ff
      font-size 24px
    .count
      color rgb(147,153,159)
      display inline-block
      width 24px
      line-height 24px
      text-align center
      vertical-align top
</style>
