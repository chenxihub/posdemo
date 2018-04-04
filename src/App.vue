<template>
  <div id="app">
    <!--leftTab-->
    <LeftNav></LeftNav>
    <div class="right-content">
      <router-view/>
    </div>
  </div>
</template>

<script>
  import LeftNav from '@/components/common/LeftNav'

  export default {
    name: 'App',
    components: {
      LeftNav
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

  .right-content {
    background-color: #FFF;
    position: absolute;
    left: 80px;
    height: 100%;
    width: 95%;
  }
</style>
