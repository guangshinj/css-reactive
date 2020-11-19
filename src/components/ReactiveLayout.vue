<template>
  <div class='reactive-layout-wrapper'>
    <p>Media Query为不同的屏幕定制不同的布局和样式</p>
    <p>为了将导航和内容区完全独立，只允许内容区出现滚动条，内容区之上不允许出现竖向滚动条</p>
    <p>纵向盒子分层时不用margin-top或者margin-bottom，防止BFC击穿父容器，造成外部异常的外边距;另外，浏览器默认的样式也是需要覆盖的</p>
    <div class="examples">
      <div class="example1">
        <div class="left">固定容器</div>
        <div class="right">自适应容器</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ReactiveLayout',
  data: function () {
    return {

    }
  },
  methods: {

  }
}
</script>

<style scoped lang="stylus">
.reactive-layout-wrapper
  .examples
    .example1
      display: flex
      justify-content: space-around
      .left, .right
        width: 100px
        height: 120px
        background-color: #eee
      .left
        flex: none
        background: #ddd
      .right
        flex-grow: 1
</style>
