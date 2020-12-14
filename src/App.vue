<template>
  <div id="app"
       @dblclick="showDiv">
    <div class="v-show-test"
         v-show="show">v-show初始化就会挂载该节点，通过css display显隐；<br />v-if初始化不一定挂载节点，显隐会增加或者删除节点;</div>
    <Navigator />
  </div>
</template>

<script>
import Navigator from './components/Navigator.vue'

export default {
  name: 'App',
  data () {
    return {
      show: false
    }
  },
  components: {
    Navigator
  },
  methods: {
    showDiv () {
      this.show = !this.show
    }
  }
}
</script>

<style lang="stylus">
html
  height: 100%
  body
    font-size: 0.75em
    margin: 0
    padding: 0
    height: inherit
    *
      padding-inline-start: 0
      margin-block-start: 0
      margin-block-end: 0
    p
      margin: 0
      padding: 2px 0 10px
    #app
      font-family: Avenir, Helvetica, Arial, sans-serif
      -webkit-font-smoothing: antialiased
      -moz-osx-font-smoothing: grayscale
      text-align: center
      color: #2c3e50
      height: inherit
      .v-show-test
        background: #eee
        padding: 10px
        line-height: 20px
        text-align: left
</style>
