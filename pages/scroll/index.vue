<template>
  <scroll-view >
    <view class="zb-container-body"
          @touchstart="touchstartEvent"
          @touchmove="touchmoveEvent"
          @touchend="touchendEvent"
          :change:prop="propObserver"
    >

      <div class="zb-content" :style="{ transform: translateY, transition: transition }">
        <div class="vue-pull-refresh-msg">
          <template v-if="loading">
            <svg t="1497367491334"
                 viewBox="0 0 1024 1024"
                 class="vue-pull-refresh-loading"
                 style="width: 1.2em; height: 1.2em;vertical-align: middle;fill: currentColor;overflow: hidden;"
                 version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="1977">
              <path d="M486.75754 135.400013 402.25163 59.310554C388.509379 46.936957 387.96346 25.139993 400.208308 11.540621 412.822131-2.468343 433.957671-4.001381 447.930113 8.579401L601.089596 146.484825C605.090636 150.087331 607.97301 154.488612 609.74682 159.253816 614.767405 170.908986 613.043155 184.73657 603.956951 194.827778L466.051527 347.987261C453.677999 361.729512 431.880966 362.275431 418.281663 350.030583 404.27263 337.416761 402.739592 316.281152 415.320374 302.308778L504.712387 203.028852C300.714847 206.912339 136.539841 373.49343 136.539841 578.419721 136.539841 785.780633 304.639089 953.87988 512 953.87988 720.609624 953.87988 887.460159 790.22062 887.460159 584.090467 887.460159 517.424512 870.092464 453.371077 837.556793 396.914498 828.144212 380.58164 833.754269 359.710766 850.087128 350.298184 866.420054 340.885671 887.29086 346.49566 896.703442 362.828587 935.174114 429.583765 955.725642 505.37934 955.725642 584.090467 955.725642 828.220806 758.019723 1022.145363 512 1022.145363 266.937086 1022.145363 68.274358 823.482635 68.274358 578.419721 68.274358 341.828486 253.439157 148.484867 486.75754 135.400013Z" p-id="1978" fill="#666666">
              </path>
            </svg>
            正在加载
          </template>
          <template v-else>
            <svg t="1497366759944"
                 viewBox="0 0 1024 1024"
                 :class="{'icon-reverse': flag}"
                 style="width: 1rem; height: 1rem; vertical-align: middle;fill: currentColor;overflow: hidden;"
                 version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="1040">
              <path d="M921.6 563.2c-9.6-9.6-25.6-9.6-35.2 0L544 896l0-822.4c0-12.8-9.6-22.4-25.6-22.4s-25.6 9.6-25.6 22.4L492.8 896l-342.4-339.2c-9.6-9.6-25.6-9.6-35.2 0-9.6 9.6-9.6 22.4 0 32l384 377.6c6.4 6.4 12.8 6.4 19.2 6.4 0 0 0 0 0 0 3.2 0 3.2 0 6.4 0 0 0 0 0 3.2 0 3.2 0 6.4-3.2 9.6-6.4l380.8-371.2C931.2 588.8 931.2 572.8 921.6 563.2z" p-id="1041" fill="#666666">
              </path>
            </svg>
            {{msg}}
          </template>
        </div>
        <div class="item-child" v-for="item in arr" > hello</div>
      </div>
    </view>
  </scroll-view>
</template>
<script>
export default {
  data(){
    return{
      arr:[1,2,3,4,5,6,7,8,9,10,11,1,1,1,1,1,1,1],
      downHight: 0, //下拉刷新: 容器高度
      startPoint:{},
      flag: 0, // 表示是否达到刷新条件
      isDownReset:1,
      loading: 0,  // 表示是否正在刷新中
      msg: '',
    }
  },
  computed:{
    // 过渡
    transition() {
      return this.isDownReset ? 'transform 330ms' : '';
    },
    translateY() {
      return this.downHight > 0 ? 'translateY(' + this.downHight + 'px)' : ''; // transform会使fixed失效,需注意把fixed元素写在mescroll之外
    },
  },
  methods:{
    getPoint(e){
      if (!e) {
        return {x: 0,y: 0}
      }
      if (e.touches && e.touches[0]) {
        return {x: e.touches[0].pageX,y: e.touches[0].pageY}
      } else if (e.changedTouches && e.changedTouches[0]) {
        return {x: e.changedTouches[0].pageX,y: e.changedTouches[0].pageY}
      } else {
        return {x: e.clientX,y: e.clientY}
      }
    },
    disabled(){

    },
    touchstartEvent(e,ins){
      this.downHight = 0
      this.startPoint = this.getPoint(e)
      // console.log(e,x);
    },
    touchmoveEvent(e){
      var curPoint = this.getPoint(e); // 当前点
      var moveY = Math.floor(curPoint.y - this.startPoint.y); // 和起点比,移动的距离,大于0向下拉,小于0向上拉
      console.log('curPoint',moveY)
      if(moveY>0){

        if(moveY<200){
          this.downHight = moveY
          if(moveY>80){
            this.msg = '释放刷新'
            this.flag = 1
          }else{
            this.msg = '下拉刷新'
          }
        }
        console.log('curPoint',curPoint,this.startPoint)
      }
    },
    touchendEvent(){
      if(this.flag && this.downHight > 0){
        this.downHight = 50
        this.loading = 1
        setTimeout(()=>{
          this.flag = 0
          this.loading = 0
          this.downHight = 0
        },3000)
        return
      }

      this.downHight = 0
    },
    propObserver(e){
      console.log('===',e)
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
.zb-content{
  //transition: 330ms;
  -webkit-overflow-scrolling: touch;
}
.vue-pull-refresh-msg {
  margin-top: -50px;
  height: 50px;
  text-align: center;
  line-height: 50px;
  color: #666;
  border-bottom: 1px solid #eee;
}
.vue-pull-refresh-msg .icon-reverse {
  transform: rotate(180deg);
  transition: all .3s ease;
}
.vue-pull-refresh-loading {
  -webkit-animation:loadRotate 1s linear infinite;
  animation:loadRotate 1s linear infinite;
}
@-webkit-keyframes loadRotate{
  from{
    -webkit-transform: rotate(0deg);
  }
  to{
    -webkit-transform: rotate(360deg);
  }
}
@keyframes loadRotate{
  from{
    transform: rotate(0deg);
  }
  to{
    transform: rotate(360deg);
  }
}
</style>
