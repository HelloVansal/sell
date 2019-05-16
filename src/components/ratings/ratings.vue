<!--  -->
<template>
  <div class="ratings">
    <div class="score-service">
      <div class="score-wrap">
        <div class="score">{{seller.score}}</div>
        <div class="score-title">综合评分</div>
        <div class="score-desc">高于周边商家{{seller.rankRate}}%</div>
      </div>
      <div class="service-wrap">
        <div class="service-score">
          <span class="service-attitude">服务态度</span>
          <star :size="36" :score="seller.serviceScore"></star>
          <span class="score">{{seller.score}}</span>
        </div>
        <div class="food-score">
          <span class="food-service">食物评分</span>
          <star :size="36" :score="seller.foodScore"></star>
          <span class="score">{{seller.foodScore}}</span>
        </div>
        <div class="time">
          <span class="arrive-desc">送达时间</span>
          <span class="arrive-time">{{seller.deliveryTime}}分钟</span>
        </div>
      </div>
    </div>
    <split></split>
    <div class="ratingSelect-wrap">
      <ratingSelect></ratingSelect>
    </div>
    <ul class="ratings-wrap">
      <li class="rating" v-for="(rating,index) of ratings" :key="index">
        <div class="avatar-wrap">
          <img class="avatar" :src="rating.avatar" width="28px" height="28px" alt="avatar">
        </div>
        <div class="rating-contnet-wrap">
          <div class="username-and-date">
            <span class="username">{{rating.username}}</span>
            <span class="rate-time">{{rating.rateTime}}</span>
          </div>
          <div class="star-wrap">
            <star :size="36" :score="rating.score"></star>
            <span class="arrive-time" v-if="rating.deliveryTime">{{rating.deliveryTime}}分钟送达</span>
          </div>
          <div class="text" v-if="rating.text">{{rating.text}}</div>
          <div class="recommend-wrap">
            <span class="iconfont recommend" v-if="rating.rateType===0">&#xe60a;</span>
            <span class="iconfont bad" v-if="rating.rateType===1">&#xe606;</span>
            <span
              class="recommend-content"
              v-for="(content,index) of rating.recommend"
              :key="index"
            >{{content}}</span>
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import star from 'components/star/star'
import split from 'components/split/split'
import ratingSelect from 'components/ratingSelect/ratingSelect'
import data from 'common/data'

// const ERR_OK = 0

export default {
  props: {
    seller: Object
  },
  data () {
    return {
      ratings: []
    }
  },
  components: {
    star,
    split,
    ratingSelect
  },
  created () {
    // this.$http.get('/api/ratings').then((response) => {
    //   response = response.body
    //   if (response.errno === ERR_OK) {
    //     this.ratings = response.data
    //   }
    // }
    // )
    this.ratings = data.ratings
  }
}

</script>
<style lang='stylus' scoped>
@import '../../common/stylus/mixin'
.ratings
  .score-service
    display: flex
    .score-wrap
      flex: 11
      margin: 18px 0
      text-align: center
      border-right: 1px solid rgba(7, 17, 27, 0.1)
      .score
        font-size: 24px
        color: rgb(255, 153, 0)
        line-height: 28px
        margin-bottom: 6px
      .score-title
        font-size: 12px
        color: rgb(7, 17, 27)
        line-height: 12px
        margin-bottom: 8px
      .score-desc
        font-size: 10px
        color: rgb(147, 153, 159)
        line-height: 10px
        margin-bottom: 6px
    .service-wrap
      flex: 19
      padding: 18px 24px
      font-size: 0
      text-align: left
      .service-score, .food-score, .time
        margin-bottom: 8px
        .service-attitude, .food-service, .arrive-desc
          font-size: 12px
          color: rgb(7, 17, 27)
          line-height: 18px
          margin-right: 12px
          vertical-align: top
        .star-wrap
          display: inline
          vertical-align: top
        .score
          font-size: 12px
          color: rgb(255, 153, 0)
          line-height: 18px
          vertical-align: top
          margin-left: 12px
        .arrive-time
          font-size: 12px
          color: rgb(147, 153, 159)
          line-height: 18px
  .ratingSelect-wrap
    margin: 0 18px
  .ratings-wrap
    padding: 0px 18px 18px 18px
    border-top: 1px solid rgba(7, 17, 27, 0.1)
    .rating
      display: flex
      margin-top: 18px
      border-1px(rgba(7, 17, 27, 0.1))
      .avatar-wrap
        width: 28px
        height: 28px
        margin-right: 12px
        .avatar
          border-radius: 50%
  .rating-contnet-wrap
    flex: 1
    font-size: 0
    .username-and-date
      display: flex
      justify-content: space-between
      margin-bottom: 4px
      .username
        font-size: 10px
        color: rgb(7, 17, 27)
        line-height: 12px
      .rate-time
        font-size: 10px
        font-weight: 200
        color: rgb(147, 153, 159)
        line-height: 12px
    .star-wrap
      display: inline-block
      margin-bottom: 6px
      .star, .arrive-time
        font-size: 10px
        font-weight: 200
        color: rgb(147, 153, 159)
        line-height: 12px
        vertical-align: text-bottom
        margin-left: 6px
    .text
      font-size: 12px
      color: rgb(7, 17, 27)
      line-height: 18px
      margin-bottom: 8px
    .recommend-wrap
      padding-bottom: 18px
      .recommend, .bad
        margin-right: 8px
        font-size: 16px
        color: rgb(0, 160, 220)
        line-height: 18px
      .bad
        color: rgb(183, 187, 191)
      .recommend-content
        display: inline-block
        padding: 0 6px
        margin-right: 8px
        font-size: 9px
        color: rgb(147, 153, 159)
        line-height: 16px
        border: 1px solid rgba(7, 17, 27, 0.1)
        vertical-align top
</style>
