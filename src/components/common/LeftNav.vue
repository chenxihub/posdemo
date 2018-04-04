<template>
    <div class="left-nav">
      <ul>
        <li>
          <i class="icon iconfont icon-ditu"></i>
          <p>收银</p>
        </li>
        <li>
          <i class="icon iconfont icon-changyi"></i>
          <p>店铺</p>
        </li>
        <li>
          <i class="icon iconfont icon-yueyeche"></i>
          <p>商品</p>
        </li>
        <li>
          <i class="icon iconfont icon-feiji"></i>
          <p>会员</p>
        </li>
        <li>
          <i class="icon iconfont icon-dongche"></i>
          <p>统计</p>
        </li>
      </ul>
    </div>
</template>

<script>
    export default {
        name: "left-nav"
    }
</script>

<style scoped>
  .left-nav{
    background-color: #409EFF;
    width: 80px;
    height: 100%;
    position: fixed;
    left: 0;
    top: 0;
  }
  ul, li {
    list-style: none;
    -webkit-padding-start: 0px;
  }
  .left-nav ul li{
    display: inline-block;
    cursor: pointer;
    padding-bottom: 20px;
  }
  .left-nav ul li i {
    font-size: 44px;
    color: #fff;
  }

  .left-nav ul li p {
    font-size: 14px;
    color: #fff;
    -webkit-margin-before: 0em;
    -webkit-margin-after: 0em;
  }
</style>
