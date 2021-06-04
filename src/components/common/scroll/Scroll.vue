<template>
 <div class="wrapper" ref="wrapper">
   <div class="content">
     <slot></slot>
   </div>
 </div>
</template>

<script>
import BScroll from 'better-scroll'
export default {
  name: 'Scroll',
  props:{
    probeType:{
      type:Number,
      default:0
    },
    pullUpLoad:{
      type:Boolean,
      default:false
    }
  },
  data(){
    return{
      scroll:null //存放better-scroll对象
    }
  },
  methods: {
    scrollTo(x,y,time=300){
      this.scroll.scrollTo(x,y,time)
    },
    finishPullUp(){
      this.scroll.finishPullUp()
    }
  },
  mounted(){
    //创建BScroll事件
    this.scroll = new BScroll(this.$refs.wrapper,{
      click:true,
      probeType:this.probeType,
      pullUpLoad:this.pullUpLoad,
      observeDOM:true,
      mouseWheel:true
    }),
    //监听滚动的位置
    this.scroll.on('scroll',(position) =>{
      // console.log(position);
      this.$emit('scroll',position)
    }),
    this.scroll.on('pullingUp',()=>{
      this.$emit('pullingUp')
    })
  }

}

</script>

<style scoped>

</style>
