<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>

<script>
import Vue from 'vue'
import CustomInput from './components/CustomInput'
import Border from './components/Border'
import BorderLast from './components/BorderLast'
import PropertyStore from './models/PropertyStore'
import autosize from 'autosize'

Vue.component('CustomInput', CustomInput)

export default {
  name: 'App',
  components: {
    Border,
    BorderLast,
    CustomInput
  },
  data: function () {
    return {
      sharedState: PropertyStore
    }
  }
}
</script>

<style lang="scss">
body{
    font-family: -apple-system, 'BlinkMacSystemFont', 'Roboto', 'Hiragino Kaku Gothic ProN', 'Noto Sans JP', sans-serif;
    margin: 0 !important;
    box-sizing: border-box;
}

::-webkit-scrollbar{
  width: 0px;
}

textarea{
  font-family: -apple-system, 'BlinkMacSystemFont', 'Roboto', 'Hiragino Kaku Gothic ProN', 'Noto Sans JP', sans-serif;
}

.text{
    width: 100%;
    max-width: 800px;
    margin: 0 auto;
}
#app{
  width: 100%;
}

.md-overlay{
  background: rgba(255, 255, 255, 0.8) !important;
}
.md-dialog{
  background: #fff;
  box-shadow: none !important;
  border: 1px solid;
  border-radius: 5px !important;
  border-color: rgba(0, 0, 0, 0.1)
}
.md-dialog-content{
  color: rgba(0, 0, 0, 0.8)
}
.md-dialog-title{
  color: rgba(0, 0, 0, 0.8)
}
.md-list-item-content{
  color: rgba(0, 0, 0, 0.6)
}
#delete{
  .md-list-item-content{
    color: #c62828
  }
}
#login-page{
  position: absolute;
  width: 100%;
  background-color: #fff;
  z-index: 5;
}
</style>
