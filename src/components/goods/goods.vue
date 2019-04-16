<template>
  <div class="goods">
    <div class="goods-wrap">
      <div class="menu-wrap" ref="menuWrap">
        <ul class="menu">
          <li
            class="menu-item border-1px"
            v-for="(item, index) of goods"
            :key="index"
            :class="{'current':currentIndex === index}"
            @click="foodMove(index,$event)"
          >
            <span class="text">
              <span v-if="item.type>0" class="icon" :class="classMap[item.type]"></span>
              <span>{{item.name}}</span>
            </span>
          </li>
        </ul>
      </div>
      <div class="main-wrap" ref="mainWrap">
        <div>
          <ul class="main food-list-hook" v-for="(good, index) of goods" :key="index">
            <div class="title">{{good.name}}</div>
            <ul class="foods-wrap" ref="foods">
              <li
                class="food-item border-1px food-item-hook"
                v-for="(item, index) of good.foods"
                :key="index"
              >
                <div class="food-img-wrap">
                  <img :src="item.image" width="60" height="60">
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
                  <div class="cartControl-wrap">
                    <cartControl :food="item" :index="index" @cartAdd="cartDrop"></cartControl>
                  </div>
                </div>
              </li>
            </ul>
          </ul>
        </div>
      </div>
    </div>
    <shopCart
      class="shopCart"
      ref="shopCart"
      :selectFoods="selectFoods"
      :deliveryPrice="seller.deliveryPrice"
      :minPrice="seller.minPrice"
    ></shopCart>
  </div>
</template>

<script>
import BScroll from 'better-scroll'
import shopCart from 'components/shopCart/shopCart'
import cartControl from 'components/cartControl/cartControl'

const ERR_OK = 0

export default {
  props: {
    seller: Object
  },
  components: {
    shopCart,
    cartControl
  },
  data () {
    return {
      goods: [],
      listHeight: [],
      scrollY: 0
    }
  },
  created () {
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
    this.$http.get('/api/goods').then((response) => {
      response = response.body
      if (response.errno === ERR_OK) {
        this.goods = response.data
        // 由于vue中数据更新是异步的，在dom解构没有加载完成，BScroll无法获取目标容器的高度，会出现无法滚动的现象，vue中的$nextTick()可以解决这个问题，在页面数据变化完成后才执行的函数需要写在$nextTick中 。
        this.$nextTick(() => {
          this._initScroll()
          this._calculateHeight()
        })
      }
    }
    )
  },
  computed: {
    // 计算当Y值在listHeight列表的某个区间时，返回这个标题的索引，将这个索引绑定到menu-wrap下的所有标签，当这个索引等于所有标签中的某个索引时，将这个标签绑定class="current"，就可以设置样式了
    currentIndex () {
      for (let i = 0; i < this.listHeight.length; i++) {
        let height1 = this.listHeight[i]
        let height2 = this.listHeight[i + 1]
        if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
          return i
        }
      }
      return 0
    },
    selectFoods () {
      let foods = []
      this.goods.forEach((good) => {
        good.foods.forEach((food) => {
          if (food.count) {
            foods.push(food)
          }
        })
      })
      return foods
    }
  },
  methods: {
    // 初始化2个滚动条
    // better-scroll会将点击事件去掉，传入click: true后这个滚动项中的子标签才能被点击
    // probeType设置为3可以监听scroll
    _initScroll () {
      this.menuScroll = new BScroll(this.$refs.menuWrap, {
        click: true
      })
      this.mainScroll = new BScroll(this.$refs.mainWrap, {
        click: true,
        probeType: 3
      })
      // 实时计算当前滚动的Y值
      this.mainScroll.on('scroll', (pos) => {
        this.scrollY = Math.abs(Math.round(pos.y))
      })
    },
    // 获取每个标题的Y值，存储于listHeight
    _calculateHeight () {
      let foodList = this.$refs.mainWrap.getElementsByClassName('food-list-hook')
      let height = 0
      this.listHeight.push(height)
      for (let i = 0; i in foodList; i++) {
        height += foodList[i].clientHeight
        this.listHeight.push(height)
      }
    },
    foodMove (index, event) {
      // 在非移动端模式下，better-scroll也派发了一个click事件，所以有2个事件，使用event._constructed将js原生事件return掉
      // better-scroll派发的event事件和原生js的event有属性上的区别，其中有一个属性为event._constructed。better-scroll派发的事件中event._constructed为true，原生点击事件中没有这个属性。
      if (!event._constructed) {
        return
      }
      // better-scroll有一个接口scrollToElement可以操作滚动到指定元素,第二个参数为滚动动画时间
      let food = this.$refs.mainWrap.getElementsByClassName('food-list-hook')[index]
      this.mainScroll.scrollToElement(food, 300)
    },
    // 子组件传过来的一个事件接收一个target参数
    cartDrop (target) {
      // 体验优化，异步执行下落动画
      this.$nextTick(() => {
        this.$refs.shopCart.drop(target)
      })
    }
  }
}

</script>

<style lang='stylus' scoped>
@import '../../common/stylus/mixin'
.goods
  .goods-wrap
    display: flex
    position: absolute
    width: 100%
    top: 174px
    left: 0
    bottom: 47px
    overflow: hidden
    .menu-wrap
      display: flex
      flex-shrink: 0
      flex-direction: column
      width: 80px
      background-color: #f3f5f7
      .current
        z-index: 10
        margin-top: -1px
        font-weight: 700
        background-color: #fff
        &.text
          border-none()
      .menu-item
        display: table
        width: 100%
        padding: 0 12px
        box-sizing: border-box
        height: 54px
        .text
          display: table-cell
          vertical-align: middle
          font-size: 12px
          line-height: 14px
          color: rgb(24, 20, 20)
          border-1px(rgba(7, 17, 27, 0.1))
        .icon
          display: inline-block
          width: 14px
          height: 14px
          font-size: 14px
          vertical-align: top
          background-size: 14px 14px
          background-repeat: no-repeat
          &.decrease
            bg-img: decrease_3
          &.discount
            bg-img: discount_3
          &.guarantee
            bg-img: guarantee_3
          &.special
            bg-img: special_3
          &.invoice
            bg-img: invoice_3
    .main-wrap
      flex: 1
      width: 100%
      .main
        .title
          width: 100%
          height: 26px
          padding-left: 8px
          font-size: 12px
          line-height: 26px
          border-left: 3px solid #d9dde1
          color: rgb(147, 153, 159)
          background-color: #f3f5f7
        .foods-wrap
          width: 100%
          box-sizing: border-box
          padding: 0 18px
          .food-item
            display: flex
            position: relative
            padding: 18px 0
            box-sizing: border-box
            border-1px: rgba(7, 17, 27, 0.1)
            &:last-child
              border-none()
            .food-img-wrap
              display: inline-block
              margin-right: 10px
              vertical-align: top
            .food-content
              flex: 1
              margin-top: 2px
              .name
                font-size: 14px
                color: rgb(7, 17, 27)
                line-height: 14px
                margin-bottom: 8px
              .desc, .desc2
                font-size: 10px
                color: rgb(147, 153, 159)
                margin-bottom: 8px
              .desc
                line-height: 14px
              .rating
                margin-left: 6px
              .price-all
                font-size: 0
                .price-icon, .price, .old-price
                  font-size: 10px
                  color: rgb(147, 153, 159)
                  font-weight: normal
                  line-height: 24px
                .price-icon
                  color: red
                .price
                  font-size: 14px
                  font-weight: 700
                  color: red
                .old-price
                  margin-left: 8px
                  text-decoration: line-through
              .cartControl-wrap
                position: absolute
                right: 0
                bottom: 12px
</style>
