<template>
  <div id="app" ref="blur">
    <v-header :seller="seller"></v-header>
    <div class="tab border-1px">
      <router-link to="/goods" class="tab-item">商品</router-link>
      <router-link to="/ratings" class="tab-item">评价</router-link>
      <router-link to="/seller" class="tab-item">商家</router-link>
    </div>
    <router-view :seller="seller"></router-view>
  </div>
</template>

<script>
import header from 'components/header/header'

const ERR_OK = 0

export default {
  name: 'App',
  components: {
    'v-header': header
  },
  data () {
    return {
      seller: {}
    }
  },
  created () {
    this.$http.get('/api/seller').then((response) => {
      response = response.body
      if (response.errno === ERR_OK) {
        this.seller = response.data
      }
    })
  }

}
</script>

<style lang='stylus' scoped>
@import 'common/stylus/mixin.styl'
#app
  .tab
    display: flex
    width: 100%
    height: 40px
    line-height: 40px
    color: rgb(77, 85, 93)
    border-1px: rgba(7, 17, 27, 0.1)
    .tab-item
      flex: 1
      font-size: 14px
      text-align: center
    .active
      color: rgb(240, 20, 20)
</style>
