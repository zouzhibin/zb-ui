<template>
  <view class="zb-container-body">
    <div class="item-child" v-for="item in arr" > hello</div>
  </view>
</template>
<script>
export default {
  data(){
    return{
      arr:[1,2,3,4,5,6,7,8,9,10,11,1,1,1,1,1,1,1]
    }
  }
}
</script>
<style lang="scss" scoped>
.zb-container-body{
  position: relative;
  height: auto;
  overflow: hidden;
  box-sizing: border-box;
  .item-child{
    height: 50px;
    line-height: 50px;
    border: 1px solid red;
  }
}
</style>
