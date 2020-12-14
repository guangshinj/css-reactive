<template>
  <div class='font-unit-wrapper'>
    <p class="rem-guide">这句话的字体大小通过设置1 rem随HTML的字体而变化, 你可以修改HTML标签字体大小来感受下：
      <input v-model="htmlFontSize">
    </p>
    <hr>
    <p><strong>当前html标签的font-size为:</strong><span>{{htmlFontSize}}</span></p>
    <p><strong>当前body标签及子孙容器的默认font-size为:</strong><span>{{bodyFontSize}}</span></p>
    <p><strong>当前屏幕DPI为:</strong><span>{{dpi}}</span></p>
    <hr>
    <p><strong>字体大小的极限:</strong><span> 除了设置字体大小为0之外, 浏览器渲染字体最小是12px, 任何单位转换px小于12都是无效的</span></p>
    <hr>
    <p><strong class="red">特别说明: 下面阅读到单位, 用大写表示单位（比如PX）, 用小写表示具体的值（比如dpi）</strong><span></span></p>
    <div class="format">
      <p><strong>PX单位说明:</strong><span>PX是固定长度单位, 1 PX = 72/dpi PT = {{72/dpi}} PT</span></p>
      <p><strong>PT单位说明:</strong><span>PT是固定长度单位, 1 PT = 1/72 DPI = dpi/72 PX = {{Number(dpi/72).toFixed(4)}} PX</span></p>
      <p><strong>%单位说明:</strong><span>%是相对长度单位, 相对于父标签的font-size, 1 % = 0.01 EM = {{bodyFontSizeValue / 100}} PX</span></p>
      <p><strong>EM单位说明:</strong><span>EM是相对长度单位, 相对于父标签的font-size, 1 EM = {{bodyFontSizeValue}} {{bodyFontSizeUnit.toUpperCase()}}</span></p>
      <p><strong>REM单位说明:</strong><span>REM是相对长度单位, 相对于html标签的font-size, 1 REM = {{htmlFontSizeValue}} {{htmlFontSizeUnit.toUpperCase()}}</span></p>
      <p><strong>VW单位说明:</strong><span>1 VW 等于视口宽度的 1 %</span></p>
      <p><strong>VH单位说明:</strong><span>1 VH 等于视口高度的 1 %</span></p>
      <p><strong>IN单位说明:</strong><span>IN表示英尺, 1 IN = dpi PX = {{dpi}} PX = 2.54 CM</span></p>
      <p><strong>PC单位说明:</strong><span>PC大约12PT, 1 PC = 1/6 DPI = 12 PT = {{dpi/6}} PX</span></p>
    </div>
    <hr>
    <div class="examples">
      <div class="example"
           :key="key"
           v-for="(value, key) in examples">
        <div class="setup">
          <input @keyup="changeSize($event, value)"
                 :name="value.key"
                 v-model="value.size">
          <label :for="value.key">{{value.unit}}</label>
        </div>

        <span :style="{fontSize:value.size + value.unit}">example</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'FontUnit',
  data: function () {
    return {
      dpi: 1,
      bodyFontSize: '16px',
      htmlFontSize: '16px',
      examples: {
        pxSize: { size: 12, unit: 'PX' },
        ptSize: { size: 9, unit: 'PT' },
        pcSize: { size: 0.75, unit: 'PC', step: 0.1 },
        inSize: { size: 1 / 16, unit: 'IN', step: 0.1 },
        percentSize: { size: 100, unit: '%' },
        remSize: { size: 0.75, unit: 'REM', step: 0.1 },
        emSize: { size: 1, unit: 'EM', step: 0.1 },
        vwSize: { size: 2, unit: 'VW' },
        vhSize: { size: 2, unit: 'VH' }
      }
    }
  },
  watch: {
    htmlFontSize () {
      this.changeHTMLFontSize()
    }
  },
  computed: {
    htmlFontSizeUnit () {
      return this.htmlFontSize.replace(/\d|\./g, '')
    },
    bodyFontSizeUnit () {
      return this.bodyFontSize.replace(/\d|\./g, '')
    },
    htmlFontSizeValue () {
      return this.htmlFontSize.replace(this.htmlFontSizeUnit, '')
    },
    bodyFontSizeValue () {
      return this.bodyFontSize.replace(this.bodyFontSizeUnit, '')
    }
  },
  mounted () {
    this.readBodyFontSize()
    const arr = this.getDPI()
    console.info(arr)
    this.dpi = arr[0]
  },
  methods: {
    getDPI () {
      const arrDPI = new Array(2)
      if (window.screen.deviceXDPI !== undefined) {
        arrDPI[0] = window.screen.deviceXDPI
        arrDPI[1] = window.screen.deviceYDPI
      } else {
        var tmpNode = document.createElement('DIV')
        tmpNode.style.cssText = 'width:1in;height:1in;position:absolute;left:0px;top:0px;z-index:99;visibility:hidden'
        document.body.appendChild(tmpNode)
        arrDPI[0] = parseInt(tmpNode.offsetWidth)
        arrDPI[1] = parseInt(tmpNode.offsetHeight)
        tmpNode.parentNode.removeChild(tmpNode)
      }
      return arrDPI
    },
    readBodyFontSize () {
      this.bodyFontSize = getComputedStyle(document.body).fontSize
    },
    changeHTMLFontSize () {
      document.querySelector('html').style.fontSize = this.htmlFontSize
    },
    changeSize ($event, config) {
      const step = config.step || 1
      switch ($event.which) {
        case 38: {
          config.size = Number(config.size) + step
          break
        }
        case 40: {
          config.size = Number(config.size) - step
          break
        }
      }
    }
  }
}
</script>

<style scoped lang="stylus">
.font-unit-wrapper
  .format
    p
      strong
        display: inline-block
        width: 80px
        text-align: right
      span
        width: calc(100% - 100px)
  input
    width: 80px
    border: 2px solid #ccc
  p
    text-align: left
    color: #333
    .red
      color: red
    strong
      display: inline-block
      color: #222
      padding-right: 10px
      vertical-align: top
      line-height: 16px
    span
      line-height: 16px
      display: inline-block
    b
      padding: 5px
    &.rem-guide
      font-size: 1rem
  .examples
    display: flex
    flex-wrap: wrap
    label
      margin-left: 4px
    .example
      border: 1px solid #eee
      margin: 0 0 10px 10px
      padding: 8px
      .setup
        margin-bottom: 10px
        input
          width: 46px
      span
        border: 1px solid #555
</style>
