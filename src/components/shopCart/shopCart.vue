<template>
  <div class="shopCart">
    <div class="shopCartImg-wrap" @click="funcShopCartListShow">
      <div class="shopCart-img" :class="{'Count-blue':totalCount}">
        <div class="iconfont" :class="{'Count-white':totalCount}">&#xe607;</div>
      </div>
      <div class="count-wrap" v-show="totalCount">
        <span class="count">{{totalCount}}</span>
      </div>
    </div>
    <div
      class="price"
      :class="{'price-white':totalPrice}"
      @click="funcShopCartListShow"
    >￥{{totalPrice}}</div>
    <div class="delivery-fee" @click="funcShopCartListShow">另需配送费￥{{deliveryPrice}}元</div>
    <div class="start-fee" :class="{settlement: totalPrice>=20}" @click="settlement">
      <span :class="{'settlement-white': totalPrice>=20}">{{payDesc}}</span>
    </div>
    <div class="ball-wrap">
      <transition
        name="drop"
        @before-enter="beforeDrop"
        @enter="dropping"
        @after-enter="afterDrop"
        v-for="(ball,index) of this.balls"
        :key="index"
      >
        <div v-show="ball.show" class="ball">
          <div class="inner inner-hook"></div>
        </div>
      </transition>
    </div>
    <div class="shopCart-list border-1px" v-show="listShow">
      <div class="list-title-wrap">
        <span class="list-title">购物车</span>
        <span class="clear" @click="funcClear">清空</span>
      </div>
      <div class="shopCart-content-wrap" ref="listContent">
        <div class="shopCart-content-scroll-wrap">
          <div class="shopCart-content" v-for="(food,index) of selectFoods" :key="index">
            <span class="shopCart-name">{{food.name}}</span>
            <span class="shopCart-price-wrap">
              <span class="shopCart-price-logo">￥</span>
              <span class="shopCart-price">{{food.price*food.count}}</span>
            </span>
            <span class="cartControl-wrap" @click="funcCartControl(index,$event)">
              <cartControl :food="food" :index="index"></cartControl>
            </span>
          </div>
        </div>
      </div>
    </div>
    <div class="shopCart-list-bg" v-show="listShow" @click="funcShopCartListShow"></div>
  </div>
</template>

<script>
import BScroll from 'better-scroll'
import cartControl from 'components/cartControl/cartControl'

export default {
  name: 'shopCart',
  props: {
    deliveryPrice: {
      type: Number,
      default: 0
    },
    minPrice: {
      type: Number,
      default: 0
    },
    selectFoods: {
      type: Array,
      default: function () {
        return []
      }
    }
  },
  components: {
    cartControl
  },
  data () {
    return {
      balls: [
        { show: false },
        { show: false },
        { show: false },
        { show: false },
        { show: false }
      ],
      dropBalls: [],
      listShow: false
    }
  },
  computed: {
    totalPrice () {
      let total = 0
      this.selectFoods.forEach((food) => {
        total += food.price * food.count
      })
      return total
    },
    totalCount () {
      let count = 0
      this.selectFoods.forEach((food) => {
        count += food.count
      })
      return count
    },
    payDesc () {
      if (this.totalPrice === 0) {
        return `￥${this.minPrice}起送`
      } else
      if (this.totalPrice < this.minPrice) {
        return `还差￥${this.minPrice - this.totalPrice}起送`
      } else {
        return '去结算'
      }
    }
  },
  methods: {
    drop (el) {
      for (let i = 0; i < this.balls.length; i++) {
        let ball = this.balls[i]
        if (ball.show === false) {
          ball.show = true
          ball.el = el
          this.dropBalls.push(ball)
          return
        }
      }
    },
    beforeDrop (el) {
      let count = this.balls.length
      while (count--) {
        let ball = this.balls[count]
        if (ball.show) {
          // 获得点击元素在视口的位置rect
          let rect = ball.el.getBoundingClientRect()
          // 获得点击元素和购物车之间x向距离
          let x = rect.left - 32
          // 获得点击元素和购物车之间y向距离
          let y = -(window.innerHeight - rect.top - 22)
          el.style.display = ''
          el.style.webkitTransform = `translate3d(0,${y}px,0)`
          el.style.transform = `translate3d(0,${y}px,0)`
          let inner = el.getElementsByClassName('inner-hook')[0]
          inner.style.webkitTransform = `translate3d(${x}px,0,0)`
          inner.style.transform = `translate3d(${x}px,0,0)`
        }
      }
    },
    dropping (el, done) {
      /* eslint-disable no-unused-vars */
      let rf = el.offsetHeight
      this.$nextTick(() => {
        el.style.webkitTransform = 'translate3d(0,0,0)'
        el.style.transform = 'translate3d(0,0,0)'
        let inner = el.getElementsByClassName('inner-hook')[0]
        inner.style.webkitTransform = 'translate3d(0,0,0)'
        inner.style.transform = 'translate3d(0,0,0)'
        el.addEventListener('transitionend', done)
      })
    },
    afterDrop (el) {
      let ball = this.dropBalls.shift()
      if (ball) {
        ball.show = false
        el.style.display = 'none'
      }
    },
    funcShopCartListShow () {
      if (this.totalCount) {
        this.listShow = !this.listShow
        this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new BScroll(this.$refs.listContent, {
              click: true
            })
          } else {
            this.scroll.refresh()
          }
        })
      }
    },
    funcClear () {
      this.selectFoods.forEach((food) => {
        food.count = 0
      })
      this.listShow = false
    },
    funcCartControl (index, event) {
      if (event.target.className === 'iconfont add') {
        this.selectFoods[index].count++
      }
      if (event.target.className === 'iconfont inner') {
        this.selectFoods[index].count--
        if (!this.totalCount) {
          this.listShow = false
        }
      }
    },
    settlement () {
      if (this.totalPrice > this.minPrice) {
        window.alert(`支付${this.totalPrice + 4}元`)
        this.selectFoods.forEach((food) => {
          food.count = 0
        })
        this.listShow = false
      }
    }
  }
}
</script>

<style lang="stylus" scoped>
@import '../../common/stylus/mixin'
.shopCart
  display: flex
  position: fixed
  z-index: 50
  left: 0
  width: 100%
  bottom: 0
  height: 47px
  color: rgba(255, 255, 255, 0.4)
  background-color: #141d27
  font-size: 0
  .shopCartImg-wrap
    position: relative
    display: inline-block
    top: -11px
    width: 56px
    height: 56px
    margin: 0 12px 0 12px
    border-radius: 50%
    background-color: #141d27
    .shopCart-img
      margin: 6px 0 0 6px
      display: inline-block
      width: 44px
      height: 44px
      background-color: rgba(255, 255, 255, 0.2)
      border-radius: 50%
      &.Count-blue
        background-color: rgb(0, 160, 220)
      .iconfont
        font-size: 24px
        text-align: center
        margin-top: 10px
        &.Count-white
          color: #fff
    .count-wrap
      position: fixed
      left: 44px
      bottom: 41px
      width: 24px
      height: 12px
      border-radius: 6px
      text-align: center
      background-color: rgb(240, 20, 20)
      .count
        font-size: 9px
        font-weight: 700
        color: #fff
        line-height: 12px
        box-shadow: 0px 4px 8px 0 rgba(0, 0, 0, 0, 4)
  .price
    margin-top: 11px
    margin-bottom: 12px
    height: 24px
    line-height: 24px
    font-size: 16px
    vertical-align: top
    padding-right: 12px
    border-right: 1px solid rgba(255, 255, 255, 0.1)
    font-weight: 700
    &.price-white
      color: #fff
  .delivery-fee
    flex: 1
    margin-left: 12px
    height: 47px
    line-height: 47px
    font-size: 10px
  .start-fee
    width: 105px
    height: 47px
    line-height: 47px
    padding: 0 8px
    font-size: 12px
    font-weight: 700
    text-align: center
    background-color: rgba(255, 255, 255, 0.1)
    &.settlement
      background-color: #00b43c
    .settlement-white
      color: #fff
      font-size: 20px
  .ball-wrap
    .ball
      position: fixed
      left: 32px
      bottom: 22px
      z-index: 200
      transition: all 0.4s cubic-bezier(0.49, -0.29, 0.75, 0.41)
      .inner
        width: 16px
        height: 16px
        border-radius: 50%
        background-color: rgb(0, 160, 220)
        transition: all 0.4s linear
  .shopCart-list
    position: absolute
    left: 0
    bottom: 47px
    width: 100%
    z-index: -1
    font-size: 0
    backdrop-filter: blur(10px)
    .list-title-wrap
      padding: 0 18px
      border-1px: rgba(7, 12, 27, 0.1)
      background-color: #f3f5f7
      backdrop-filter: blur(10px)
      .list-title
        font-size: 14px
        font-weight: 200
        color: rgb(7, 17, 27)
        height: 40px
        line-height: 40px
      .clear
        float: right
        font-size: 12px
        color: rgb(0, 160, 200)
        line-height: 40px
        border: none
        background-color: none
  .shopCart-list-bg
    position: fixed
    top: 0
    left: 0
    right: 0
    bottom: 0
    background-color: rgba(7, 17, 27, 0.6)
    z-index: -5
  .shopCart-content-wrap
    padding: 0 18px
    max-height: 217px
    overflow: hidden
    background-color: #fff
    backdrop-filter: blur(10px)
    // &::-webkit-scrollbar
    // width: 0
    .shopCart-content
      display: flex
      margin: 12px 0
      .shopCart-name
        flex: 2
        font-size: 14px
        line-height: 36px
        height: 35px
        color: rgb(7, 17, 27)
      .shopCart-price-wrap
        margin: 0 12px 0 18px
        color: rgb(240, 20, 20)
        line-height: 36px
        height: 36px
        .shopCart-price-logo
          font-size: 10px
          font-weight: normal
        .shopCart-price
          font-size: 14px
          font-weight: 700
</style>
