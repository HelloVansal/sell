<template>
  <div class="cartControl">
    <transition name="fade">
      <span class="cart-decrease" v-show="food.count>0" @click="decreaseCart($event)">
        <span class="iconfont inner">&#xe60c;</span>
      </span>
    </transition>
    <span class="num" v-show="food.count>0">{{food.count}}</span>
    <span class="iconfont add" @click="addCart($event)">&#xe60b;</span>
    <div class="ball-wrap">
      <transition
        name="drop"
        class="ball"
        v-show="ball.show"
        v-for="(ball,index) of this.balls"
        :key="index"
      >
        <div class="inner"></div>
      </transition>
    </div>
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
  data () {
    return {
      balls: [
        { show: false },
        { show: false },
        { show: false },
        { show: false },
        { show: false }
      ]
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
  .ball-wrap
    .ball
      display: inline-block
      position: fixed
      left: 32px
      bottom: 22px
      z-index: 200
      .inner
        width: 16px
        height: 16px
        border-radius: 50%
        background-color: rgb(0, 160, 220)
</style>
