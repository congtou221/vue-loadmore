<template>
  <div id="app">
    <transition
    v-on:after-enter="afterEnter"
    v-on:before-leave="beforeLeave"
    >
      <div class="up-loader" v-show="!upFinish">
        <span class="loader-text"></span>
      </div>
    </transition>

    <a class="button" @click="goPrevious">go previous</a>
    <List :page="page" @hide="hideLoading"></List>
    <a class="button" @click="goNext">go next<span>current:{{page}}</span></a>

    <transition
    v-on:before-enter="beforeEnter"
    v-on:enter="enter"
    v-on:after-enter="afterEnter"
    v-on:before-leave="beforeLeave"
    >
      <div class="down-loader" v-show="!downFinish">
        <span class="loader-text"></span>
      </div>
    </transition>
  </div>
</template>

<script>
import $ from 'jquery'
import List from './components/List'

export default {
  components: {
    List
  },
  data () {
    return {
      page: 1,
      upFinish: true,
      downFinish: true,
      up: true,
      showLoading: false
    }
  },
  methods: {
    goNext () {
      this.downFinish = false
      this.up = true
      this.page++
    },
    goPrevious () {
      this.upFinish = false
      this.up = false
      if (this.page === 1) {
        return
      }
      this.page--
    },
    beforeEnter: function (el) {
      // 想写成类似iscroll的效果，奈何这里的loading一直不显示啊，不知道vue内部是怎么控制钩子函数的执行的
      $(el).parent().append('<div class="loading">loading</div>')
    },
    enter: function (el) {
      $('.loading').remove()
    },
    afterEnter: function (el) {
      $(el).find('.loader-text').text('正在加载')
    },
    beforeLeave: function (el) {
      $(el).find('.loader-text').text('加载完毕')
    },
    hideLoading () {
      setTimeout(() => {
        this.upFinish = this.downFinish = true
      }, 200)
    },
    sleep (ms) {
      ms += new Date().getTime()
      while (new Date() < ms) {

      }
    }
  }
}
</script>

<style scoped>
.button {
  display: block;
  width: 100%;
  background: #212121;
  color: #fff;
  font-weight: bold;
  text-align: center;
  padding: 1em;
  cursor: pointer;
  text-decoration: none;
}
.button span {
  margin-left: 2em;
  font-size: .5rem;
  color: #d6d6d6;
}
.up-loader, .down-loader {
  position: fixed;
  width: 100%;
  padding: 1rem;
  background-color: #212121;
}
.up-loader{
  top: 0;
}
.down-loader{
  height: 5rem;
  bottom: 0;
  color: #ffffff;
}
.loader-text{
    font-size: 5rem;
    background-color: #212121;
    color: #ffffff;
    position: absolute;
    width: 100%;
    text-align: center;
}
.loading{
  position: fixed;
  bottom: 0;
  height: 5rem;
  width: 100%;
  background: green;
  color:red;
}
</style>
