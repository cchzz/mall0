<template>
    <swiper>
      <swiper-item v-for="item in banners">
        <a :href="item.link"></a>
        <img :src="item.image" alt="">
      </swiper-item>
    </swiper>
</template>

<script>
import {Swiper,SwiperItem} from 'components/common/swiper'

export default {
  name: 'HomeSwiper',
  props:{
    banners:{
      type:Array,
      dafault(){
        return[]
      }
    }
  },
  components:{
    Swiper,
    SwiperItem
  }
}

</script>

<style scoped>

</style>
