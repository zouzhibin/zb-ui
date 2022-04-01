<template>
 <view >
   <view style="height: 100%">
     <template v-if="item.type==='selection'">
     </template>
     <template v-else>
       {{ item.label }}
       <view class="sorter-table" v-if="item.sorter">
         <view :class="['sorter-table-icon',item.sorterMode==='_asc'&&`sorting${item.sorterMode||''}`]"></view>
         <view :class="['sorter-table-icon',item.sorterMode==='_desc'&&`sorting${item.sorterMode||''}`]"></view>
       </view>
     </template>
   </view>

   <view v-if="item.children && item.children.length" class="parent-item">
     <table-header
         v-for="(it,index) in item.children"
         :border="border"
         :item="it"
     />
   </view>
 </view>
</template>
<script>
export default {
  name:'table-header',
  props:{
    item:{
      type:Object,
      default:()=>{}
    },
    // transColumns:{
    //   type:Array,
    //   default:()=>[]
    // },
    // scrollbarSize:{
    //   type:Number,
    //   default:0
    // },
    border:{
      type:Boolean,
      default:false
    },
  },
  methods:{
    sortAction(){
      this.$emit('sortAction',item,index)
    }
  }
}
</script>

<style>
.parent-item{
  width: 100%;
  display: flex;
}
</style>
