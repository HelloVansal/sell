<template>
  <div class="shopCart">
    <div class="shopCartImg-wrap">
      <div class="shopCart-img" :class="{'Count-blue':totalCount}">
        <div class="iconfont" :class="{'Count-white':totalCount}">&#xe607;</div>
      </div>
      <div class="count-wrap" v-show="totalCount">
        <span class="count">{{totalCount}}</span>
      </div>
    </div>
    <div class="price" :class="{'price-white':totalPrice}">￥{{totalPrice}}</div>
    <div class="delivery-fee">另需配送费￥{{deliveryPrice}}元</div>
    <div class="start-fee" :class="{settlement: totalPrice>=20}">
      <span :class="{'settlement-white': totalPrice>=20}">{{payDesc}}</span>
    </div>
  </div>
</template>

<script>
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
        return [
          {
            price: 3,
            count: 20
          }
        ]
      }
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
  right: 0
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
      background-color: rgb(0, 160, 220)
    .settlement-white
      color #fff
      font-size 20px
</style>
