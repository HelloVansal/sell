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
  .bulletin
    position relative
    height 28px
    padding-right 24px
    //overflow auto
    background-color rgba(7, 17, 27, 0.2)
    //overflow hidden
    // text-overflow ellipsis
    // white-space nowrap
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
