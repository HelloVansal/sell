<template>
  <div class="goods">
    <ul class="menu-wrap">
      <li class="menu-item border-1px" v-for="(item, index) of goods" :key="index">
        <span class="text">
          <span v-if="item.type>0" class="icon" :class="classMap[item.type]"></span>
          <span>{{item.name}}</span>
        </span>
      </li>
    </ul>
    <div class="main-wrap">
      <ul class="main" v-for="(good, index) of goods" :key="index">
        <div class="title-wrap">
          <div class="title">{{good.name}}</div>
        </div>
        <ul class="foods-wrap">
          <li class="food-item border-1px" v-for="(item, index) of good.foods" :key="index">
            <div class="food-img-wrap">
              <img :src="item.image" width="57" height="57">
            </div>
            <div class="food-content">
              <div class="name">{{item.name}}</div>
              <div class="desc" v-if="item.description">{{item.description}}</div>
              <div class="desc2">
                <span class="sellCount">月售{{item.sellCount}}份</span>
                <span class="rating">好评率{{item.rating}}%</span>
              </div>
              <div class="price-all">
                <span class="price-icon">￥</span>
                <span class="price">{{item.price}}</span>
                <span class="old-price" v-if="item.oldPrice">￥{{item.oldPrice}}</span>
              </div>
              <div class="add">
                <span class="iconfont" v-show="numShow">&#xe60c;</span>
                <span class="num" v-show="numShow">{{num}}</span>
                <span class="iconfont" @click="funcAdd(item,index)">&#xe60b;</span>
              </div>
            </div>
          </li>
        </ul>
      </ul>
    </div>
  </div>
</template>

<script>
const ERR_OK = 0

export default {
  data () {
    return {
      goods: {},
      numShow: false,
      num: 0
    }
  },
  methods: {
    funcAdd (item, index) {
      console.log(item, index)
      // item[index].num++
      // item[index].numShow = true
    }
  },
  created () {
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
    this.$http.get('/api/goods').then((response) => {
      response = response.body
      if (response.errno === ERR_OK) {
        this.goods = response.data
      }
    }
    )
  }
}

</script>

<style lang='stylus' scoped>
@import '../../common/stylus/mixin'
.goods
  display flex
  position absolute
  width 100%
  top 174px
  left 0
  bottom 47px
  overflow hidden
  .menu-wrap
    display flex
    flex-shrink 0
    flex-direction column
    width 80px
    overflow auto
    background-color #f3f5f7
    &::-webkit-scrollbar
      width 0
    // &::-webkit-scrollbar-thumb
    // background-color #d9dde1
    // &::-webkit-scrollbar-track
    // background-color blue
    .menu-item
      display table
      width 100%
      padding 0 12px
      box-sizing border-box
      height 54px
      &:hover
        background-color #fff
      .text
        display table-cell
        vertical-align middle
        font-size 12px
        line-height 14px
        color rgb(24, 20, 20)
        border-1px(rgba(7, 17, 27, 0.1))
      .icon
        display inline-block
        width 14px
        height 14px
        font-size 14px
        vertical-align top
        background-size 14px 14px
        background-repeat no-repeat
        &.decrease
          bg-img decrease_3
        &.discount
          bg-img discount_3
        &.guarantee
          bg-img guarantee_3
        &.special
          bg-img special_3
        &.invoice
          bg-img invoice_3
  .main-wrap
    flex 1
    width 100%
    overflow-x hidden
    overflow-y auto
    &::-webkit-scrollbar
      width 0
    .main
      .title-wrap
        padding-left 3px
        background-color #d9dde1
        .title
          width 100%
          height 26px
          padding-left 11px
          font-size 12px
          line-height 26px
          color rgb(147, 153, 159)
          background-color #f3f5f7
      .foods-wrap
        width 100%
        box-sizing border-box
        padding 0 18px
        .food-item
          display flex
          position relative
          padding 18px 0
          box-sizing border-box
          border-1px (rgba(7, 17, 27, 0.1))
          &:last-child
            border-none()
          .food-img-wrap
            display inline-block
            margin-right 10px
            vertical-align top
          .food-content
            flex 1
            min-width 180px
            margin-top 2px
            .name
              font-size 14px
              color rgb(7, 17, 27)
              height 14px
              line-height 14px
              margin-bottom 8px
            .desc, .desc2
              font-size 10px
              color rgb(147, 153, 159)
              width 80%
              margin-bottom 8px
              overflow hidden
              white-space nowrap
              text-overflow ellipsis
              .rating
                margin-left 12px
            .price-all
              font-size 0
              .price-icon, .price, .old-price
                font-size 10px
                color rgb(147, 153, 159)
                font-weight normal
                line-height 24px
              .price-icon
                color red
              .price
                font-size 14px
                font-weight 700
                color red
              .old-price
                margin-left 8px
                text-decoration line-through
            .add
              position absolute
              right 0
              bottom 18px
              font-size 0
            .num
              display inline-block
              width 24px
              font-size 10px
              line-height 24px
              text-align center
              color rgb(147, 153, 159)
            .iconfont
              vertical-align top
              font-size 20px
              line-height 24px
</style>
