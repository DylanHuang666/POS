<template>
  <div class="left-nav">
    <ul>
      <li>
        <i class="iconfont icon-money"></i>
        <div>收银</div>
      </li>
      <li>
        <i class="iconfont icon-canyindianmeishidian"></i>
        <div>店铺</div>
      </li>
      <li>
        <i class="iconfont icon-food"></i>
        <div>美食</div>
      </li>
      <li>
        <i class="iconfont icon-huiyuan"></i>
        <div>会员</div>
      </li>
      <li>
        <i class="iconfont icon-cart"></i>
        <div>统计</div>
      </li>
      <li>
        <i class="iconfont icon-shezhi"></i>
        <div>设置</div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'LeftNav',
  
}
</script>


<style>
  .left-nav{
    height: 100%;
    color:#fff;
    background-color: #1d8ce0;
    float: left;
    width: 5%;
    font-size: 10px;
  }
  .iconfont{
    font-size: 24px;
  }
  .left-nav li{
    text-align: center;
    border-bottom: 1px solid #20a0ff;
    padding: 10px;
  }
</style>
