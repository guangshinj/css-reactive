<template>
  <div class='bfc-wrapper'>
    <h1>非BFC容器产生的问题或现象</h1>
    <div class="btn-group">
      <div>
        <label>ul变成BFC：</label>
        <button :key="index"
                v-for="(item,index) in bfcButtons"
                class="click"
                @click="changeStyle(item)">{{item.css}}</button>
      </div>
      <div>
        <label>ul变成非BFC：</label>
        <button :key="index"
                v-for="(item,index) in unbfcButtons"
                class="click"
                @click="changeStyle(item)">{{item.css}}</button>
      </div>
    </div>
    <h2>1. 顶部元素的margin-top或底部元素的margin-bottom可能击穿非BFC容器</h2>
    <p>说明：</p>
    <ul>
      <ol>1. 灰色区域是容器，设置display: inline-block，成为BFC</ol>
      <ol>2. 蓝色区域是ul，它是<b>{{isBFC?'':'非'}}BFC</b></ol>
      <ol>3. 绿色区域是多个li，默认display: list-item，它不是BFC</ol>
    </ul>
    <div class="example1">
      <ul :style="ulStyle">
        <li class="margin">height:60px;margin-top:10px;margin-bottom:10px;</li>
        <li class="margin">height:60px;margin-top:10px;margin-bottom:10px;</li>
      </ul>
    </div>
    <h2>2. 非BFC容器之间margin重叠</h2>
    <div class="example1">
      <ul ref="ul">
        <li class="margin"
            :style="{display:inlineBlock?'inline-block':'block'}">height:60px;margin-top:10px;margin-bottom:10px;</li>
        <li class="margin"
            :style="{display:inlineBlock?'inline-block':'block'}">height:60px;margin-top:10px;margin-bottom:10px;</li>
      </ul>
    </div>
    <button class="click"
            @click="inlineBlock = !inlineBlock">{{inlineBlock?'block':'inline-block'}}</button>

    <h2>3. 非BFC容器无法被浮动元素撑开</h2>
    <div class="wrapper">
      <div class="yellow"
           :style="ulStyle">
        我是<b>{{isBFC?'':'非'}}BFC</b>容器
        <div class="float">
          float div，我是绿色
        </div>
      </div>
    </div>

    <h2>4. 浮动元素被文字环绕</h2>
    <div class="wrapper3">
      <div class="float">float div</div>
      <div :style="ulStyle"
           class="right">{{content}}</div>
    </div>

  </div>
</template>

<script>
export default {
  name: 'bfc',
  data: function () {
    return {
      isBFC: false,
      inlineBlock: false,
      bfcButtons: [
        { isBFC: true, css: 'overflow:hidden' },
        { isBFC: true, css: 'overflow:scroll' },
        { isBFC: true, css: 'overflow:auto' },
        { isBFC: true, css: 'float:right;clear:both' },
        { isBFC: true, css: 'float:left;left:30px;' },
        { isBFC: true, css: 'display:inline-block' },
        { isBFC: true, css: 'display:flex' },
        { isBFC: true, css: 'display:table' },
        { isBFC: true, css: 'display:table-cell' },
        { isBFC: true, css: 'position:absolute' },
        { isBFC: true, css: 'position:fixed;bottom:10px;' }
      ],
      unbfcButtons: [
        { isBFC: false, css: 'overflow:visible' },
        { isBFC: false, css: 'float:none' },
        { isBFC: false, css: 'position:relative' },
        { isBFC: false, css: 'position:static' },
        { isBFC: false, css: 'position:sticky' },
        { isBFC: false, css: 'display:block' }
      ],
      ulStyle: {
        display: 'block'
      },
      cssStyle: {},
      wrapperWidth: 0
    }
  },
  computed: {
    content () {
      return this.isBFC ? '当前部分BFC可以实现左右布局'.repeat(50) : '一些文字'.repeat(50)
    }
  },
  mounted () {
    document.body.onresize = (e) => {
    }
  },
  methods: {
    changeStyle ({ css, isBFC }) {
      const styles = css.split(';')
      const ulStyle = {}
      for (const style of styles) {
        if (style.length) {
          const lang = style.split(':')
          ulStyle[lang[0]] = lang[1]
        }
      }
      this.ulStyle = ulStyle
      this.isBFC = isBFC
    }

  }
}
</script>

<style lang="stylus">
.bfc-wrapper
  text-align: left
  .btn-group
    position: sticky
    top: 1px
    background: #aaa
    z-index: 20
  h2
    background-color: red
    margin: 20px 0 10px 0
  p
    &.round
      text-indent: 20px
      color: purple
      font-weight: 600
  .click
    margin: 10px 10px 10px 0
  .example1
    position: relative
    display: inline-block
    width: 40%
    height: 160px
    background: gray
    color: #eee
    .measure-height
      writing-mode: lr-tb
      text-align: left
      width: 20px
      position: absolute
      word-break: break-all
      height: 100%
      display: flex
      justify-content: center
      align-items: center
      background: red
      box-sizing: border-box
      padding: 0 4px 0 4px
      font-size: 12px
      text-align: center
    ul
      margin-left: 30px
      width: calc(100% - 30px)
      height: 140px
      background: blue
      &:before
        content: '高度140px'
        @extend .measure-height
        height: 140px
      .margin
        height: 40px
        background: green
        margin: 10px 0 10px 20px
        list-style-type: none
    &:before
      content: '固定高度 160px'
      @extend .measure-height
  .float
    float: left
    left: 40px
    width: 50%
    height: 40px
    background: green
  .wrapper
    position: relative
    display: inline-block
    .yellow
      width: 100%
      background: yellow
  .wrapper3
    position: relative
    width: 40%
    height: 160px
    background: yellow
    .right
      background-color: purple
      color: #fff
</style>
