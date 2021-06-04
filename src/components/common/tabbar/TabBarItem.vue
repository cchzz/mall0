<template>
  <div class="tab-bar-item" @click="itemClick">
    <div v-if="!isActive"><slot name="item-icon"></slot></div>
    <div v-else><slot name="item-icon-active"></slot></div>
    <div :style="activeStyle"><div><slot name="item-text"></slot></div></div>
    </div>
</template>

<script>
export default {
  name: 'TabBarItem',
  props:{
    path:String,
    activeColor:{
      type:String,
      // default:'yellow'
    }
  },
  data(){
    return{
      // isActive:true
    }
  },
  computed:{
    isActive(){
      return this.$route.path.indexOf(this.path) !== -1
    },
    activeStyle(){
      return this.isActive ? {color: this.activeColor} : {}
    }
  },
  methods:{
    itemClick(){
      this.$router.replace(this.path).catch((err)=>console.log(err))
    }
  }
}

</script>

<style scoped>
.tab-bar-item {
  flex: 1;
  /* float: left; */
  text-align: center;
  height: 49px;
  font-size: 14px;
}
.tab-bar-item img {
  width: 24px;
  height: 24px;
  vertical-align: middle;
  margin-top: 3px;
  /* margin-bottom: 2px; */
}
/* .active{
  color: red;
} */
</style>
