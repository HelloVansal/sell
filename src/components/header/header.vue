<template>
  <div class="header">
    <div class="content-wrap">
      <div class="avatar">
        <img width="64" height="64" :src="seller.avatar">
      </div>
      <div class="content">
        <div class="content-title">
          <div></div>
          <span>{{seller.name}}</span>
        </div>
        <div class="description">{{seller.description}} / {{seller.deliveryTime}}分钟送达</div>
        <div class="supports">
          <div v-if="seller.supports" :class="classMap[seller.supports[0].type]"></div>
          <span v-if="seller.supports">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div v-if="seller.supports" class="support-count" @click="funcdetailShow()">
        <span class="count">{{seller.supports.length}}个</span>
        <span class="iconfont">&#xe603;</span>
      </div>
    </div>
    <div class="bulletin-wrap" @click="funcdetailShow()">
      <div class="bulletin-img"></div>
      <span class="bulletin-text">{{seller.bulletin}}</span>
      <div class="iconfont">&#xe603;</div>
    </div>
    <div class="bgImg">
      <img :src="seller.avatar">
    </div>
    <div v-show="detailShow" class="detail">
      <div class="detail-wrap">
        <div class="detail-name">{{seller.name}}</div>
        <div class="star-wrap">
          <span class="star" :class="item" v-for="(item, index) of this.starname" :key="index"></span>
        </div>
        <div class="detail-title">
          <div class="line"></div>
          <span class="detail-title-text">优惠信息</span>
          <div class="line"></div>
        </div>
        <div class="detail-content-wrap">
          <div class="detail-content" v-for="item of seller.supports" :key="item.type">
            <div class="detail-img" :class="classMap[item.type]"></div>
            <span v-if="seller.supports" class="detail-content">{{item.description}}</span>
          </div>
        </div>
        <div class="detail-title">
          <div class="line"></div>
          <span class="detail-title-text">商家公告</span>
          <div class="line"></div>
        </div>
        <div class="detail-bulletin">{{seller.bulletin}}</div>
      </div>
      <div class="iconfont" @click="funcdetailShow()">&#xe608;</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Header',
  props: {
    seller: Object,
    appdom: HTMLDivElement
  },
  data () {
    return {
      'detailShow': false,
      'detailstyle': {
        filter: 'blur(10px)'
      }
    }
  },
  methods: {
    funcdetailShow: function () {
      this.detailShow = !this.detailShow
      if (this.detailShow) {
        this.appdom.style.filter = 'blur(10px)'
      }
    }
  },
  created () {
    this.classMap = ['decrease', 'discount', 'discount', 'special', 'special']
    this.starname = ['staron', 'staron', 'staron', 'staron', 'staroff']
  }
}

</script>

<style lang='stylus' scoped>
@import '../../common/stylus/mixin'
.header
  position relative
  overflow hidden
  color rgb(255, 255, 255)
  background-color rgba(7, 17, 27, 0.5)
  .content-wrap
    position relative
    height 106px
    box-sizing border-box
    padding 24px 12px 18px 24px
    .avatar
      float left
      >img
        border-radius 4px
    .content
      margin-left 16px
      display inline-block
      font-size 0
      position relative
      .content-title
        margin 2px 0 8px 0
        >div
          display inline-block
          width 30px
          height 18px
          bg-img brand
          background-size 100%
          background-repeat no-repeat
          vertical-align top
        >span
          margin-left 6px
          font-size 16px
          line-height 18px
          font-weight bold
      .description
        margin-bottom 10px
        font-size 12px
      .supports
        >div
          display inline-block
          width 12px
          height 12px
          background-size 100%
          background-repeat no-repeat
          vertical-align top
        .decrease
          bg-img decrease_1
        .discount
          bg-img discount_1
        .guarantee
          bg-img guarantee_1
        .special
          bg-img special_1
        .invoice
          bg-img invoice_1
        >span
          margin-left 4px
          font-size 10px
          line-height 12px
    .support-count
      position absolute
      height 24px
      right 12px
      padding 0 4px 0 8px
      bottom 15px
      border-radius 12px
      background-color rgba(0, 0, 0, 0.2)
      >span:nth-child(1)
        display inline-block
        font-size 10px
        line-height 24px
      >span:nth-child(2)
        line-height 24px
        vertical-align top
      .supports-icon
        display inline-block
        padding 7px 8px
        position absolute
        right 0
        bottom -6px
        border-radius 16px
        background-color rgba(0, 0, 0, 0.2)
        div
          font-size 10px
          line-height 12px
  .bulletin-wrap
    position relative
    height 28px
    line-height 28px
    background-color rgba(7, 17, 27, 0.2)
    padding 0 32px 0 4px
    overflow hidden
    white-space nowrap
    text-overflow ellipsis
    .bulletin-img
      display inline-block
      width 22px
      height 12px
      bg-img(bulletin)
      background-size 22px 12px
      background-repeat no-repeat
      vertical-align top
      margin-top 8px
    .bulletin-text
      font-size 10px
      vertical-align top
      margin 0 4px
    .iconfont
      position absolute
      right 12px
      top 0
  .bgImg
    position absolute
    left 0
    right 0
    top 0
    bottom 0
    filter blur(10px)
    z-index -1
    img
      width 100%
      height 100%
  .detail
    position fixed
    z-index 100
    box-sizing border-box
    top 0
    left 0
    width 100%
    bottom 0
    text-align center
    overflow auto
    background-color rgba(7, 17, 27, 0.8)
    padding 64px 30px 32px 30px
    .detail-wrap
      min-height 100%
      box-sizing border-box
      padding-bottom 32px
      .detail-name
        margin-bottom 16px
        font-size 16px
        font-weight 700
      .star-wrap
        height 24px
        margin-bottom 28px
        font-size 0
        .star
          display inline-block
          width 24px
          height 24px
          margin 0 6px
          background-size 24px 24px
          background-repeat no-repeat
        .staron
          bg-img star48_on
        .starhalf
          bg-img star48_half
        .staroff
          bg-img star48_off
      .detail-title
        height 14px
        margin-bottom 24px
        font-size 0
        .line
          display inline-block
          width 33%
          height 10px
          border-top 2px solid rgba(255, 255, 255, 0.2)
          vertical-align middle
        .detail-title-text
          font-size 14px
          font-weight 700
        .line:nth-child(1)
          margin-right 12px
        .line:nth-child(3)
          margin-left 12px
      .detail-content-wrap
        margin 0 12px 22px 12px
        .detail-content
          height 16px
          line-height 16px
          text-align left
          margin-bottom 12px
          font-size 0
          .detail-img
            display inline-block
            width 16px
            height 16px
            margin-right 6px
            vertical-align top
            background-size 16px 16px
            background-repeat no-repeat
          .decrease
            bg-img decrease_1
          .discount
            bg-img discount_1
          .guarantee
            bg-img guarantee_1
          .special
            bg-img special_1
          .invoice
            bg-img invoice_1
          .detail-content
            font-size 12px
      .detail-bulletin
        margin 0 12px 40px 12px
        font-size 12px
        line-height 24px
        text-align left
    >.iconfont
      width 32px
      height 32px
      margin -32px auto 0 auto
      font-size 32px
      color rgba(255, 255, 255, 0.5)
</style>
