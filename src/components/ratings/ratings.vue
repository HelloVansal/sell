<!--  -->
<template>
  <div class="ratings">
    <div class="score-service">
      <div class="score-wrap">{{ratings[0].score}}</div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    ratings: Array
  },
  created () {
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

  }
}

</script>
<style lang='stylus' scoped></style>
