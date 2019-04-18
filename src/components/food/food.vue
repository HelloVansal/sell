<template>
  <div class="food-wrap">
    <transition name="foodIn">
      <div class="food" v-show="foodShow">
        <div class="food-img-wrap">
          <img class="food-img" :src="food.image" alt="图片">
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
          <div class="add-cart">加入购物车</div>
        </div>
        <div class="line-wrap">
          <div class="line1 border-1px"></div>
          <div class="line2 border-1px"></div>
        </div>
        <div class="info-wrap" v-if="food.info">
          <div class="info-title">商品介绍</div>
          <div class="info">{{food.info}}</div>
        </div>
        <div class="line-wrap" v-if="food.info">
          <div class="line1 border-1px"></div>
          <div class="line2 border-1px"></div>
        </div>
        <div class="rating-wrap">
          <div class="rating-title">商品评价</div>
          <div class="rating-style-wrap border-1px">
            <div class="rating-style-all">
              全部
              <span class="count">{{allRatings}}</span>
            </div>
            <div class="rating-style-Recommend">
              推荐
              <span class="count">{{recommend}}</span>
            </div>
            <div class="rating-style-bad">
              吐槽
              <span class="count">{{allRatings-recommend}}</span>
            </div>
          </div>
          <div class="only-content-wrap">
            <span class="iconfont only-content">&#xe602;</span>
            <span class="only-content-desc">只看有内容的评价</span>
          </div>
        </div>
        <div class="rating-contents-wrap">
          <div class="rating-content-wrap" v-for="(rating,index) of food.ratings" :key="index">
            <div class="rate-time-name">
              <span class="rate-time">{{rating.rateTime}}</span>
              <span class="user-name-wrap">
                <span class="user-name">{{rating.username}}</span>
                <img :src="rating.avatar" class="avatar" width="12" height="12" alt="avatar">
              </span>
            </div>
            <div class="rating-content">
              <span class="iconfont">&#xe606;</span>
              <span class="text">{{rating.text}}</span>
            </div>
          </div>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  data () {
    return {
    }
  },
  props: {
    food: {
      type: Object
    },
    foodShow: {
      type: Boolean,
      default: false
    }
  },
  computed: {
    allRatings () {
      if (this.food.ratings) {
        return this.food.ratings.length
      }
    },
    recommend () {
      let i = 0
      if (this.food.ratings) {
        this.food.ratings.forEach(rating => {
          if (!rating.rateType) {
            i++
          }
        })
      }
      return i
    }
  },
  methods: {}
}

</script>
<style lang='stylus' scoped>
@import '../../common/stylus/mixin'
.food-wrap
  .food
    position: fixed
    top: 0
    left: 0
    right: 0
    bottom: 47px
    z-index: 30
    overflow: auto
    background-color: #fff
    &::-webkit-scrollbar
      width: 0
    &.foodIn-enter-active, &.foodIn-leave-active
      transition: all 0.5s
      opacity: 1
      transform: translate3d(0, 0, 0)
    &.foodIn-enter, &.foodIn-leave-to
      opacity: 0
      transform: translate3d(-100%, 0, 0)
    .food-img-wrap
      width: 100%
      .food-img
        width: 100%
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
    .line-wrap
      .line1, .line2
        border-1px(rgba(7, 17, 27, 0.1))
      .line2
        font-size: 10px
        padding-bottom: 16px
        background-color: #f3f5f7
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
        margin-bottom: 6px
      .rating-style-wrap
        padding: 12px 0 18px 0
        font-size: 0
        border-1px(rgba(7, 17, 27, 0.1))
        .rating-style-all, .rating-style-Recommend, .rating-style-bad
          display: inline-block
          padding: 8px 12px
          margin-right: 8px
          font-size: 12px
          line-height: 16px
          color: #fff
          border-radius: 1px
          background-color: rgb(0, 160, 220)
          .count
            font-size: 8px
        .rating-style-Recommend
          color: rgb(77, 85, 93)
          background-color: rgba(0, 160, 220, 0.2)
        .rating-style-bad
          color: rgb(77, 85, 93)
          background-color: rgba(77, 85, 93, 0.2)
      .only-content-wrap
        padding: 12px 0
        font-size: 0
        .only-content, .only-content-desc
          vertical-align: top
          margin-right: 4px
          font-size: 24px
          color: rgb(147, 153, 159)
        .only-content-desc
          font-size: 12px
          line-height: 24px
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
          .iconfont
            font-size: 20px
            vertical-align: top
            color: rgb(147, 153, 159)
          .text
            font-size: 12px
            color: rgb(7, 17, 27)
</style>
