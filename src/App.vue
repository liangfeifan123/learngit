<template>
  <div id="app">
    <leftnav></leftnav>
    <div class="main">
      <router-view></router-view>
    </div>
  </div>
</template>

<script>
//把新创建的左部导航组件引过来
import leftnav from '@/components/common/leftnav'
export default {
  name: 'app',
  components:{
    leftnav
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.main{
  float: left;
  width:95%;
  background-color:#eff2f7;
  height:100%;
  overflow: hidden;
}
</style>
