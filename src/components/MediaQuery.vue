<template>
  <div ref="mediaQuery"
       class='media-query-wrapper'>
    <h1>Media Query适应不同的屏幕</h1>
    <p>PC浏览器窗口最小宽度是500px，当前内容区域宽度是{{wrapperWidth}}px，看得出来，媒体查询监控的是整体窗口宽度</p>
  </div>
</template>

<script>
export default {
  name: 'MediaQuery',
  data: function () {
    return {
      wrapperWidth: 0
    }
  },
  mounted () {
    this.resizeWindow()
    document.body.onresize = (e) => {
      this.resizeWindow()
    }
  },
  methods: {
    resizeWindow () {
      this.wrapperWidth = this.$refs.mediaQuery.clientWidth
    }
  }
}
</script>

<style lang="stylus">
.media-query-wrapper
  background: gray
  color: #333
@media (max-width: 1280px)
  .media-query-wrapper
    background: green
    color: #aaa
@media (max-width: 800px)
  .media-query-wrapper
    background: red
    color: #ccc
@media (max-width: 500px)
  .media-query-wrapper
    background: blue
    color: #eee
</style>
