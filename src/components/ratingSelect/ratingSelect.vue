<template>
  <div class="ratingSelect">
    <div class="rating-style-wrap border-1px">
      <div class="rating-style-all" @click="select(2,$event)" :class="{'active':selectType===2}">
        全部
        <span class="count">{{ratings.length}}</span>
      </div>
      <div
        class="rating-style-Recommend"
        @click="select(0,$event)"
        :class="{'active':selectType===0}"
      >
        推荐
        <span class="count">{{recommend}}</span>
      </div>
      <div class="rating-style-bad" @click="select(1,$event)" :class="{'active':selectType===1}">
        吐槽
        <span class="count">{{ratings.length-recommend}}</span>
      </div>
    </div>
    <div class="only-content-wrap">
      <span
        class="iconfont only-content"
        :class="{'on':onlyContent}"
        @click="funcOnlyContent"
      >&#xe602;</span>
      <span class="only-content-desc">只看有内容的评价</span>
    </div>
  </div>
</template>

<script>
const RECOMMEND = 0
// const BAD = 1
const ALL = 2

export default {
  props: {
    ratings: {
      type: Array,
      default () {
        return []
      }
    },
    // selectType: {
    //   type: Number,
    //   default: ALL
    // },
    // onlyContent: {
    //   type: Boolean,
    //   default: false
    // },
    desc: {
      type: Object,
      default () {
        return {
          all: '全部',
          recommend: '推荐',
          bad: '吐槽'
        }
      }
    }
  },
  data () {
    return {
      selectType: ALL,
      onlyContent: false
    }
  },
  computed: {
    // 推荐评论数量
    recommend () {
      let i = 0
      if (this.ratings) {
        this.ratings.forEach(rating => {
          if (rating.rateType === RECOMMEND) {
            i++
          }
        })
      }
      return i
    }
  },
  methods: {
    select (type, event) {
      if (!event._constructed) {
        return
      }
      this.selectType = type
      this.$emit('selectTypeChange', type)
    },
    funcOnlyContent () {
      this.onlyContent = !this.onlyContent
      this.$emit('onlyContentChange', this.onlyContent)
    }
  }
}

</script>
<style lang='stylus' scoped>
@import '../../common/stylus/mixin'
.ratingSelect
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
      border-radius: 1px
      .count
        font-size: 8px
    .rating-style-all
      color: #000
      background-color: rgba(0, 160, 220, 0.2)
      &.active
        color: #fff
        background-color: rgb(0, 160, 220)
    .rating-style-Recommend
      color: rgb(77, 85, 93)
      background-color: rgba(0, 160, 220, 0.2)
      &.active
        color: #fff
        background-color: rgb(0, 160, 220)
    .rating-style-bad
      color: rgb(77, 85, 93)
      background-color: rgba(77, 85, 93, 0.2)
      &.active
        color: #fff
        background-color: rgb(77, 85, 93)
  .only-content-wrap
    padding: 12px 0
    font-size: 0
    .only-content, .only-content-desc
      vertical-align: top
      margin-right: 4px
      font-size: 24px
      color: rgb(147, 153, 159)
    .on
      color: rgb(0, 160, 220)
    .only-content-desc
      font-size: 12px
      line-height: 24px
</style>
