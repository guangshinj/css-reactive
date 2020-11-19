<template>
  <div class='bfc-wrapper'>
    <h1>BFC的怪异现象</h1>
    <h2>1. margin重叠</h2>
    <div class="example1">
      <ul>
        <li class="margin">1</li>
        <li class="margin">2</li>
        <li class="margin">3</li>
      </ul>
      <ul class="other">other</ul>
    </div>
    <div class="example2">
      <ul>
        <li class="margin">4</li>
        <li class="margin">5</li>
        <li class="margin">6</li>
      </ul>
      <ul class="other">other</ul>
    </div>
    <p class="round"
       v-bind:style="cssStyle">
      灰色是div（通过display:inline-block成为BFC），蓝色是ul（左侧默认不是BFC，右侧通过display:inline-block成为BFC），绿色是li（设置上下margin），可以看出：
    </p>
    <ul>
      <li>左侧的第一个li的margin-top和第三个li的margin-bottom击穿了ul，ul不能完全容纳li，影响了ul之外的布局；右侧ul是BFC，阻止了li影响外部</li>
      <li>左侧的li之间的margin发生重叠，右侧通过display: inline-block保证不重叠</li>
    </ul>
    <h2>2. 浮动</h2>
    <p>
      上面紫色的文字跟右侧的ul形成了环绕，通过下面的按钮进行调整：
    </p>
    <div class="control">
      <button class="click"
              @click="overflow">overflow:hidden;</button>
      <button class="click"
              @click="clear">clear:both;</button>
      <button class="click"
              @click="none">恢复</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'bfc',
  data: function () {
    return {
      cssStyle: {},
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
    overflow () {
      this.cssStyle = { overflow: 'hidden' }
    },
    clear () {
      this.cssStyle = { clear: 'both' }
    },
    none () {
      this.cssStyle = {}
    },
    resizeWindow () {
    }
  }
}
</script>

<style lang="stylus">
.bfc-wrapper
  text-align: left
  h2
    background-color: red
  p
    &.round
      text-indent: 20px
      color: purple
      font-weight: 600
  .example1, .example2
    display: inline-block
    width: 40%
    background: gray
    ul
      background: blue
      margin: 0 10px
      width: calc(100% - 20px)
      .margin
        height: 20px
        background: green
        margin: 10px
        list-style-type: none
      &.other
        background: red
  .example2
    margin-left: 10%
    float: right
    position: relative
    ul
      display: inline-block
      li
        display: inline-block
        width: calc(100% - 20px)
  .control
    button
      margin-left: 10px
</style>
