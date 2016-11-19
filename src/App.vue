<template>
  <div id="app">
    <transition name="up">
      <div class="up-loader" v-show="!upFinish">
        <span class="loader-text">Loading</span>
      </div>
    </transition>

    <a class="button" @click="goPrevious">go previous</a>
    <List :page="page" @hide="hideLoading"></List>
    <a class="button" @click="goNext">go next<span>current:{{page}}</span></a>

    <transition name="down">
      <div class="down-loader" v-show="!downFinish">
        <span class="loader-text">Loading</span>
      </div>
    </transition>
  </div>
</template>

<script>
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
      up: true
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
    hideLoading () {
      setTimeout(() => {
        this.upFinish = this.downFinish = true
      }, 200)
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
    background-color: #212121;
    z-index: 1;
    width: 100%;
}
.up-loader{
  top: 0;
}
.down-loader{
  bottom: 0;
}
.loader-text{
    font-size: 5rem;
    color: #ffffff;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, 50%);
}
.down-enter-active {
  animation: expand .5s 1 cubic-bezier(0, 1, 0, 1) both;
}
.down-leave-active {
  bottom: 0;
  height: auto;
  animation: collapse .5s 1 cubic-bezier(0, 1, 0, 1) both;
}
.up-enter-active {
  animation: expand .5s 1 cubic-bezier(0, 1, 0, 1) both;
}
.up-leave-active {
  top: 0;
  height: auto;
  animation: collapse .5s 1 cubic-bezier(0, 1, 0, 1) both;
}
@keyframes expand {
  0% {
    height: 3em;
    transform: translate3d(0, 0, 0);
  }
  100% {
    height: 100%;
    transform: translate3d(0, 0, 0);
  }
}
@keyframes collapse {
  0% {
    height: 100%;
    transform: translate3d(0, 0, 0);
  }
  100% {
    height: 3em;
    transform: translate3d(0, 0, 0);
  }
}

</style>
