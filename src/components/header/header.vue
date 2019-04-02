<template>
  <div class="header">
    <div class="content-wrap">
      <div class="avatar">
        <img :src="seller.avatar">
      </div>
      <div class="content">
        <div class="content-title">
          <img src="../../../resource/img/brand@2x.png"><span>{{seller.name}}</span>
        </div>
        <div class="description">{{seller.description}}</div>
        <div class="supports">
          <img src="../../../resource/img/decrease_1@2x.png"><span>{{seller.supports[0].description}}</span>
        </div>
        <span class="supports-icon">
            <div class="iconfont">5个&#xe603;</div>
        </span>
      </div>
    </div>
    <div class="bulletin">
      <img src="../../../resource/img/bulletin@2x.png" alt=""><span>{{seller.bulletin}}</span><div class="iconfont">&#xe603;</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Header',
  props: {
    seller: Object
  }
}

</script>

<style lang='stylus' scoped>
.header
  color rgb(255, 255, 255)
  .content-wrap
    height 106px
    box-sizing border-box
    padding 24px 12px 18px 24px
    background-color rgba(7, 17, 27, 0.5)
    .avatar
      float left
      margin-right 16px
      >img
        width 64px
        height 64px
        border-radius 4px
    .content
      position relative
      .content-title
        margin-top 2px
        img
          height 18px
          vertical-align top
        span
          margin-left 6px
          font-size 16px
          line-height 18px
          font-weight bold
      .description
        margin-top 8px
        font-size 12px
        font-weight 200
      .supports
        margin-top 6px
        >img
          height 12px
          vertical-align text-bottom
        >span
          margin-left 4px
          font-size 10px
          font-weight 200
          line-height 12px
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
  .bulletin
    position relative
    height 28px
    padding-right 24px
    background-color rgba(7, 17, 27, 0.2)
    white-space nowrap
    overflow hidden
    text-overflow ellipsis
    clear:both
    img
      margin-left 12px
      height 12px
      vertical-align text-bottom
    span:nth-child(2)
      margin-left 4px
      margin-right 30px
      font-size 10px
      font-weight 200
      line-height 28px
    div:nth-child(3)
      display block
      position absolute
      right 12px
      top 6px
</style>
