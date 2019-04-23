<template>
  <transition name="foodIn">
    <div class="food" v-show="foodShow" ref="food">
      <div class="food-content">
        <div class="food-img-wrap">
          <img class="food-img" :src="food.image" alt="图片">
          <div class="back-icon iconfont" @click="hide">&#xe601;</div>
        </div>
        <div class="food-detail-wrap">
          <div class="name">{{food.name}}</div>
          <div class="sell-content">
            <span class="sell-count">月售{{food.sellCount}}份</span>
            <span class="rating">好评率{{food.rating}}%</span>
          </div>
          <div class="price">
            <span class="new-price">
              <span class="sign">￥</span>
              {{food.price}}
            </span>
            <span class="old-price" v-if="food.oldPrice">￥{{food.oldPrice}}</span>
          </div>
          <div class="add-cart-wrap">
            <div
              class="add-cart"
              v-show="!food.count || food.count===0"
              @click.stop.prevent="funcCartControlShow"
            >加入购物车</div>
            <div class="cartControl-wrap">
              <cartControl v-show="food.count" :food="food" @cartAdd="cartDrop"></cartControl>
            </div>
          </div>
        </div>
        <split></split>
        <div class="info-wrap" v-if="food.info">
          <div class="info-title">商品介绍</div>
          <div class="info">{{food.info}}</div>
        </div>
        <split v-if="food.info"></split>
        <div class="rating-wrap">
          <div class="rating-title">商品评价</div>
          <ratingSelect
            :ratings="food.ratings"
            :desc="desc"
            @selectTypeChange="changeRatings"
            @onlyContentChange="changeContnet"
          ></ratingSelect>
        </div>
        <ul class="rating-contents-wrap">
          <li
            v-show="needShow(rating.rateType,rating.text)"
            class="rating-content-wrap"
            v-for="(rating,index) of food.ratings"
            :key="index"
          >
            <div class="rate-time-name">
              <span class="rate-time">{{rating.rateTime | formateDate}}</span>
              <span class="user-name-wrap">
                <span class="user-name">{{rating.username}}</span>
                <img :src="rating.avatar" class="avatar" width="12" height="12" alt="avatar">
              </span>
            </div>
            <div class="rating-content">
              <span class="iconfont bad" v-if="rating.rateType===1">&#xe606;</span>
              <span class="iconfont recommend" v-if="rating.rateType===0">&#xe60a;</span>
              <span class="text">{{rating.text}}</span>
            </div>
          </li>
        </ul>
        <div class="no-rating" v-show="!food.ratings || !food.ratings.length">暂无评价</div>
      </div>
    </div>
  </transition>
</template>

<script>
import Vue from 'vue'
import BScroll from 'better-scroll'
import cartControl from 'components/cartControl/cartControl'
import split from 'components/split/split'
import ratingSelect from 'components/ratingSelect/ratingSelect'
import { formateDate } from 'common/js/date'

// const RECOMMEND = 0
// const BAD = 1
const ALL = 2

export default {
  data () {
    return {
      foodShow: false,
      selectType: ALL,
      onlyContent: false,
      desc: {
        all: '全部',
        recommend: '推荐',
        bad: '吐槽'
      }
    }
  },
  props: {
    food: {
      type: Object
    }
  },
  components: {
    cartControl,
    split,
    ratingSelect
  },
  computed: {
  },
  filters: {
    formateDate (time) {
      let date = new Date(time)
      return formateDate(date, 'yyyy-MM-dd hh:mm')
    }
  },
  methods: {
    show () {
      this.foodShow = true
      // 初始化评论选择组件
      this.selectType = ALL
      this.onlyContent = false
      // 初始化better-scroll
      this.$nextTick(() => {
        if (!this.scroll) {
          this.scroll = new BScroll(this.$refs.food, { click: true })
        } else {
          this.scroll.refresh()
        }
      })
    },
    hide () {
      this.foodShow = false
    },
    funcCartControlShow (event) {
      if (!event._constructed) {
        return
      }
      this.$emit('cartAdd', event.target)
      Vue.set(this.food, 'count', 1)
    },
    cartDrop () {
      this.$emit('cartAdd', event.target)
    },
    changeRatings (selectType) {
      this.selectType = selectType
      // 当评论增加或减少时，要重新刷新滚动条，但要在dom挂载完之后
      this.$nextTick(() => {
        this.scroll.refresh()
      })
    },
    changeContnet (onlyContent) {
      this.onlyContent = onlyContent
      this.$nextTick(() => {
        this.scroll.refresh()
      })
    },
    needShow (type, text) {
      if (this.onlyContent && !text) {
        return false
      }
      if (this.selectType === ALL) {
        return true
      } else {
        return type === this.selectType
      }
    }
  }
}

</script>
<style lang='stylus' scoped>
@import '../../common/stylus/mixin'
.food
  position: fixed
  top: 0
  left: 0
  right: 0
  bottom: 47px
  z-index: 30
  overflow: hidden
  background-color: #fff
  &.foodIn-enter-active, &.foodIn-leave-active
    transition: all 0.3s linear
    opacity: 1
    transform: translate3d(0, 0, 0)
  &.foodIn-enter, &.foodIn-leave-to
    opacity: 0
    transform: translate3d(100%, 0, 0)
  .food-img-wrap
    position: relative
    width: 100%
    height: 0
    padding-top: 100%
    .food-img
      position: absolute
      top: 0
      left: 0
      width: 100%
      height: 100%
    .back-icon
      position: absolute
      left: 0px
      top: 0px
      padding: 20px 15px 15px 15px
      font-size: 20px
      color: #fff
  .food-detail-wrap
    position: relative
    margin: 18px
    font-size: 0
    .name
      margin-bottom: 8px
      font-size: 14px
      line-height: 14px
      font-weight: 700
      color: rgb(7, 17, 27)
    .sell-content
      margin-bottom: 18px
      .sell-count, .rating
        font-size: 10px
        line-height: 10px
        color: rgb(147, 153, 159)
      .sell-count
        margin-right: 12px
    .price
      margin-bottom: 18px
      .new-price
        font-size: 14px
        line-height: 24px
        font-weight: 700
        color: rgb(240, 20, 20)
        .sign
          font-size: 10px
          font-weight: normal
      .old-price
        text-decoration: line-through
        font-size: 10px
        line-height: 24px
        font-weight: 700
        color: rgb(147, 153, 159)
    .add-cart-wrap
      .add-cart
        position: absolute
        right: 0
        bottom: 0
        width: 74px
        height: 24px
        text-align: center
        border-radius: 12px
        font-size: 10px
        line-height: 24px
        color: #fff
        background-color: rgb(0, 160, 220)
      .cartControl-wrap
        position: absolute
        right: 0
        bottom: -6px
  .info-wrap
    margin: 18px
    .info-title
      font-size: 16px
      color: rgb(7, 17, 27)
      line-height: 16px
    .info
      margin: 6px 8px
      font-size: 12px
      line-height: 24px
      font-weight: 200
      color: rgb(77, 85, 93)
  .rating-wrap
    margin: 18px 18px 0 18px
    .rating-title
      font-size: 16px
      color: rgb(7, 17, 27)
      line-height: 16px
  .rating-contents-wrap
    padding: 0 18px
    border-top: 1px solid rgba(7, 17, 27, 0.1)
    .rating-content-wrap
      padding: 16px 0
      border-1px(rgba(rgba(7, 17, 27, 0.1)))
      .rate-time-name
        display: flex
        justify-content: space-between
        margin-bottom: 6px
        font-size: 10px
        line-height: 12px
        color: rgb(147, 153, 159)
        .user-name-wrap
          .user-name
            margin-right: 6px
          .avatar
            border-radius: 50%
      .rating-content
        .bad
          font-size: 16px
          line-height: 16px
          vertical-align: top
          color: rgb(147, 153, 159)
        .recommend
          font-size: 16px
          line-height: 16px
          vertical-align: top
          color: rgb(0, 160, 200)
        .text
          font-size: 12px
          line-height: 16px
          color: rgb(7, 17, 27)
  .no-rating
    padding: 16px
    font-size: 12px
    color: rgb(147, 153, 159)
</style>
