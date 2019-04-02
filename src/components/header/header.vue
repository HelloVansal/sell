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
        <span class="supports-icon">
            <div class="iconfont">5个&#xe603;</div>
        </span>
      </div>
      <div v-if="seller.supports" class="support-count">
        <span class="count">{{seller.supports.length}}个</span><span class="iconfont">&#xe603;</span>
      </div>
    </div>
    <div class="bulletin">
      <div></div>
      <span>{{seller.bulletin}}</span>
      <div class="iconfont">&#xe603;</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Header',
  props: {
    seller: Object
  },
  created () {
    this.classMap = ['decrease', 'discount', 'guarantee', 'special', 'invoice']
  }
}

</script>

<style lang='stylus' scoped>
@import '../../common/stylus/mixin'
.header
  color rgb(255, 255, 255)
  .content-wrap
    position relative
    height 106px
    box-sizing border-box
    padding 24px 12px 18px 24px
    background-color rgba(7, 17, 27, 0.5)
    .avatar
      float left
      >img
        border-radius 4px
    .content
<<<<<<< HEAD
      margin-left 16px
      display inline-block
      font-size 0
=======
      position relative
>>>>>>> 53e864c8f0d212dd7b347faff78a6b62797e9c9a
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
<<<<<<< HEAD
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
=======
      .supports-icon
        display inline-block
        padding 7px 8px
        position absolute
        right 0
        bottom -6px
        border-radius 16px
        background-color rgba(0,0,0,0.2)
        div
          font-size 10px
          font-weight: 200
          line-height: 12px
>>>>>>> 53e864c8f0d212dd7b347faff78a6b62797e9c9a
  .bulletin
    position relative
    height 28px
    padding-right 24px
<<<<<<< HEAD
    // overflow auto
    background-color rgba(7, 17, 27, 0.2)
    font-size 0
    // overflow hidden
    // text-overflow ellipsis
    // white-space nowrap
    >div:nth-child(1)
      display inline-block
      width 22px
=======
    background-color rgba(7, 17, 27, 0.2)
    white-space nowrap
    overflow hidden
    text-overflow ellipsis
    clear:both
    img
      margin-left 12px
>>>>>>> 53e864c8f0d212dd7b347faff78a6b62797e9c9a
      height 12px
      bg-img(bulletin)
      background-size 100%
      background-repeat no-repeat
      margin-left 12px
      vertical-align text-bottom
    >span
      margin-left 4px
      margin-right 30px
      font-size 10px
      line-height 28px
    >div:nth-child(3)
      display block
      position absolute
      right 12px
      top 6px
</style>
