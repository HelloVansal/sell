<template>
  <div class="star-wrap" :class="starType">
    <span class="star" :class="starClass" v-for="(starClass, index) of starClasses" :key="index"></span>
  </div>
</template>

<script>
export default {
  props: {
    size: Number,
    score: Number
  },
  data () {
    return {
    }
  },
  components: {},
  computed: {
    starType () {
      return 'star-' + this.size
    },
    starClasses () {
      let result = ['off', 'off', 'off', 'off', 'off']
      let intscore = Math.floor(this.score)
      let truescore = 0
      // score的小数位大于等于0.5时
      if (this.score - intscore >= 0.5) {
        truescore = intscore + 0.5
        for (let i = 0; i < truescore; i++) {
          result[i] = 'on'
        }
        result[Math.floor(truescore)] = 'half'
      } else {
        // score的小数位小于0.5时
        truescore = intscore
        for (let i = 0; i < truescore; i++) {
          result[i] = 'on'
        }
      }
      return result
    }
  }
}

</script>
<style lang='stylus' scoped>
@import '../../common/stylus/mixin'
.star-wrap
  font-size: 0
  .star
    display: inline-block
    background-repeat: no-repeat
  &.star-48
    .star
      width: 20px
      height: 20px
      margin-right: 22px
      background-size: 20px 20px
      &:last-child
        margin-right: 0
      &.on
        bg-img: 'star48_on'
      &.half
        bg-img: 'star48_half'
      &.off
        bg-img: 'star48_off'
  &.star-36
    .star
      width: 15px
      height: 15px
      margin-right: 8px
      background-size: 15px 15px
      &:last-child
        margin-right: 0
      &.on
        bg-img: 'star36_on'
      &.half
        bg-img: 'star36_half'
      &.off
        bg-img: 'star36_off'
  &.star-24
    .star
      width: 10px
      height: 10px
      margin-right: 3px
      background-size: 10px 10px
      &:last-child
        margin-right: 0
      &.on
        bg-img: 'star24_on'
      &.half
        bg-img: 'star24_half'
      &.off
        bg-img: 'star24_off'
</style>
