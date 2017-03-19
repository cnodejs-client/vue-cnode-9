<template>
  <div id="app">
    <head-comp></head-comp>
    <router-view></router-view>
  </div>
</template>

<script>

  import headComp from './components/commonpage/headComp'

  export default {
    data() {
      return {}
    },
    components: {
      headComp,
    }
  }


</script>

<style lang="less">
  @import url("./assets/css/m-reset.less");
</style>
