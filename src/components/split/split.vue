<template>
  <div class="split">
          <div class="line1 border-1px"></div>
          <div class="line2 border-1px"></div>
        </div>
</template>

<script>
export default {
}

</script>
<style lang='stylus' scoped>
@import '../../common/stylus/mixin'
.split
  width 100%
  .line1, .line2
    border-1px(rgba(7, 17, 27, 0.1))
  .line2
    font-size: 10px
    padding-bottom: 16px
    background-color: #f3f5f7
</style>
