<template>
  <div id="app">
    <Navigator />
  </div>
</template>

<script>
import Navigator from './components/Navigator.vue'

export default {
  name: 'App',
  components: {
    Navigator
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
</style>
