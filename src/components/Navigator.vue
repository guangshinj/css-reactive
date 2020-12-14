<template>
  <div class='layout-wrapper'
       :class="{theme1:leftNav}">
    <div class="header"
         v-show="showNav">
      <ul class="nav-part">
        <li @click="changeContent(config.component)"
            :key="config.name"
            v-for="(config) in navigators ">
          {{config.name}}
        </li>
      </ul>
    </div>
    <div class="content-part">
      <div class="btns">
        <button @click="switchTopNav">上方导航</button>
        <button @click="switchLeftNav">左侧导航</button>
        <button @click="triggerShowNav">{{showNav ? '隐藏' : '显示'}}导航</button>
      </div>
      <keep-alive>
        <component :is="currentComponent"></component>
      </keep-alive>
    </div>
    <div class="footer-part">
      footer
    </div>
  </div>
</template>

<script>
import Box from '@/pages/Box'
import BFC from '@/pages/BFC'
import FontUnit from '@/pages/FontUnit'
import ReactiveLayout from '@/pages/ReactiveLayout'
import MediaQuery from '@/pages/MediaQuery'

export default {
  name: 'Navigator',
  components: {
    Box,
    FontUnit,
    ReactiveLayout,
    MediaQuery
  },
  data: function () {
    return {
      leftNav: false,
      showNav: true,
      navigators: [
        { name: '响应式布局(PC)', component: ReactiveLayout },
        { name: '盒子(PC)', component: Box },
        { name: 'BFC', component: BFC },
        { name: '字体单位(PC)', component: FontUnit },
        { name: '媒体查询', component: MediaQuery }
      ],
      currentComponent: null
    }
  },
  created () {
    this.changeContent(this.navigators[0].component)
  },
  methods: {
    changeContent (component) {
      this.currentComponent = component
    },
    triggerShowNav () {
      this.showNav = !this.showNav
    },
    switchTopNav () {
      this.showNav = true
      this.leftNav = false
    },
    switchLeftNav () {
      this.showNav = true
      this.leftNav = true
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus">
.layout-wrapper
  &.theme1
    display: flex
    flex-direction: row
    flex-wrap: wrap
    .header
      display: inline-flex
      flex: none
      margin: 0
      float: left
      .nav-part
        flex-direction: column
        align-items: flex-start
        min-width: auto
        border-radius: 50px
        border-bottom-left-radius: 0
        border-top-right-radius: 0
        margin: 20px 0 20px 20px
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.05)
        li
          width: 100%
          box-sizing: border-box
          text-align: left
          font-size: 0.8em
    .content-part
      height: calc(100% - 30px)
      width: 1px
      padding: 20px 20px 0
    .footer-part
      flex: none
      width: 100%
</style>
<style lang="stylus">
.layout-wrapper
  height: inherit
  display: flex
  flex-direction: column
  ::-webkit-scrollbar
    width: 10px
    height: 10px
  ::-webkit-scrollbar-thumb
    /* 滚动条里面小方块 */
    border-radius: 10px
    background-color: skyblue
    background-image: -webkit-linear-gradient(
      45deg,
      rgba(255, 255, 255, 0.2) 25%,
      transparent 25%,
      transparent 50%,
      rgba(255, 255, 255, 0.2) 50%,
      rgba(255, 255, 255, 0.2) 75%,
      transparent 75%,
      transparent
    )
  ::-webkit-scrollbar-track
    /* 滚动条里面轨道 */
    box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.2)
    background: #ededed
    border-radius: 10px
  .header
    flex: none
    overflow-y: hidden
    overflow-x: auto
    display: inline-block
    box-sizing: border-box
    margin: 10px 20px 0
    transition: all 0.4s cubic-bezier(0.4, 0, 0, 1)
    .nav-part
      display: flex
      justify-content: center
      align-items: center
      min-width: 100%
      box-sizing: border-box
      width: fit-content
      font-size: 20px
      list-style-type: none
      background: #eee
      border-radius: 20px
      li
        display: inline-block
        color: #42b983
        cursor: pointer
        white-space: nowrap
        padding: 4px 12px
        border-radius: 20px
        &:hover
          background: #357
  .content-part
    flex-grow: 1
    margin: 0 auto
    box-sizing: border-box
    padding: 20px 20px 0
    width: 100%
    overflow: auto
    .btns
      button
        margin: 0 10px 10px 0
  .footer-part
    background-color: gray
    height: 30px
    display: flex
    justify-content: center
    align-items: center
    flex: none
  h3
    margin: 40px 0 0
</style>
