<template>
  <div id="app">
    <div class="header">
      <v-header :seller="seller" :mz="name"></v-header>
    </div>
    <div class="tab">
      <div class="tab-item"><router-link to="goods">商品</router-link></div>
      <div class="tab-item"><router-link to="ratings">评价</router-link></div>
      <div class="tab-item"><router-link to="sellers">商家</router-link></div>
    </div>
    <div class="content">
      <router-view :seller="seller"></router-view>
    </div>
  </div>
</template>

<script>
import Header from './components/header.vue'
import goods from './components/goods.vue'
import ratings from './components/ratings'
import sellers from './components/sellers'
import axios from 'axios'
export default {
  name: 'App',
  components: {
    'v-header': Header,
    goods,
    ratings,
    sellers
  },
  data () {
    return {
      seller : { score : 0.0},
      name : 'gyx'
    }
  },  
  created() {
    axios.get('/good/seller').then(res => {
      if(res.data.code === 0){
        this.seller = res.data.data
      }
    })
  }
}
</script>
<style lang='stylus' rel='stylysheet/stylus'>
  #app
    .tab
      display flex
      height 40px
      line-height 40px
      border-bottom 1px solid rgba(7,17,27,0.1)
      .tab-item
        flex 1
        text-align center
        a
          width 100%
          height 100%
          font-size 14px
          color rgb(77,85,93)
        a.active
          color rgb(240,20,20)
</style>
