<template>
  <div class='box-wrapper'>
    <p><strong>margin的宽度不在盒子范围内，但border、padding、content属于盒子</strong></p>
    <p><strong>目前html标签设置height为100%，高度占满整个屏幕</strong></p>
    <p><strong>body未设置高度，由内部元素撑开，所以高度和html不一致；body设置了左右各margin 20px，所以body比html的宽度小40px</strong></p>
    <p><strong>document盒子大小（HTML文档）:</strong> width<b>{{documentBox.width}}px</b>, height<b>{{documentBox.height}}px</b></p>
    <p><strong>body盒子大小:</strong> width<b>{{bodyBox.width}}px</b>, height<b>{{bodyBox.height}}px</b></p>
    <div class="boxs">
      <div :ref="item.className"
           :key="item.className"
           v-for="(item) in boxs"
           :class="item.className">
        <p :key="index"
           v-for="(content,index) in item.contents">
          {{content}}
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Box',
  data: function () {
    return {
      documentBox: {
        width: 0,
        height: 0
      },
      bodyBox: {
        width: 0,
        height: 0
      },
      boxs: [
        { className: 'left', contents: [] },
        { className: 'right', contents: [] }
      ]
    }
  },
  watch: {

  },
  mounted () {
    this.readBox()
    this.readDocumentBox()
    this.readBodyBox()
    document.body.onresize = (e) => {
      this.readDocumentBox()
      this.readBodyBox()
    }
  },
  methods: {
    readBox () {
      for (const box of this.boxs) {
        const contents = box.contents
        const dom = this.$refs[box.className][0]
        const cssObject = getComputedStyle(dom)
        contents.push('width: ' + cssObject.width + ', height: ' + cssObject.height)
        contents.push('padding: ' + cssObject.padding)
        let realWidth = `real width: ${dom.offsetWidth}px = ${cssObject.width}(css width) `
        if (cssObject.boxSizing === 'content-box') {
          realWidth += `+ ${cssObject.borderWidth}(border-width) * 2 + ${cssObject.paddingLeft}(padding-left) + ${cssObject.paddingRight}(padding-right)`
          contents.push('因为我是怪异盒子，超出了边界(本来2个100px高度的盒子正好放的下)')
        }
        contents.push(realWidth)
        contents.push('box-sizing: ' + cssObject.boxSizing + `，它表示css设置的宽高是给${cssObject.boxSizing === 'content-box' ? '内容' : '盒子'}设置的`)
        contents.push('border-width: ' + cssObject.borderWidth)
      }
      this.boxs = [...this.boxs]
    },
    readDocumentBox () {
      const { clientWidth, clientHeight } = document.documentElement
      this.documentBox = {
        width: clientWidth,
        height: clientHeight
      }
    },
    readBodyBox () {
      const { clientWidth, clientHeight } = document.body
      this.bodyBox = {
        width: clientWidth,
        height: clientHeight
      }
    }
  }
}
</script>

<style scoped lang="stylus">
.box-wrapper
  padding-bottom: 30px
  p
    text-align: left
    b
      margin-left: 5px
  .boxs
    height: 200px
    border: 2px solid red
    .left, .right
      width: 600px
      height: 100px
      border: 5px solid #333
      padding: 10px
      background: #eee
      p
        margin: 0
        padding: 0
    .right
      border-color: #666
      box-sizing: border-box
</style>
