<template>
  <div class="cartControl">
    <transition name="fade">
      <span class="cart-decrease" v-show="food.count>0" @click.stop.prevent="decreaseCart($event)">
        <span class="iconfont inner">&#xe60c;</span>
      </span>
    </transition>
    <span class="num" v-show="food.count>0">{{food.count}}</span>
    <span class="iconfont add" @click.stop.prevent="addCart($event)">&#xe60b;</span>
  </div>
</template>

<script>
import Vue from 'vue'

export default {
  props: {
    food: {
      type: Object
    },
    index: {
      type: Number
    }
  },
  methods: {
    addCart (event) {
      if (!event._constructed) {
        return
      }
      if (!this.food.count) {
        Vue.set(this.food, 'count', 1)
      } else {
        this.food.count++
      }
      this.$emit('cartAdd', event.target)
    },
    decreaseCart (event) {
      if (!event._constructed) {
        return
      }
      if (this.food.count) {
        this.food.count--
      }
    }
  }
}
</script>
<style lang='stylus' scoped>
.cartControl
  font-size: 0
  .cart-decrease
    display: inline-block
    padding: 6px
    .inner
      font-size: 24px
      line-height: 24px
      color: rgb(0, 160, 220)
    &.fade-enter-active, &.fade-leave-active
      transition: all 0.5s
      opacity: 1
      transform: translate3d(0, 0, 0) rotate(0)
    &.fade-enter, &.fade-leave-to
      opacity: 0
      transform: translate3d(24px, 0, 0) rotate(180deg)
  .num
    display: inline-block
    width: 12px
    font-size: 10px
    line-height: 24px
    vertical-align: top
    margin-top: 6px
    text-align: center
    color: rgb(147, 153, 159)
  .add
    display: inline-block
    padding: 6px
    font-size: 24px
    line-height: 24px
    color: rgb(0, 160, 220)
</style>
