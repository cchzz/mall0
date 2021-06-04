<template>
  <div id="home">
    <nav-bar class="home-nav"> <div slot="center">购物车</div></nav-bar>
    <scroll class="content" ref="scroll"
    :probe-type="3"
    :pull-up-load="true"
    @scroll="contentScroll"
    @pullingUp="loadMore">
      <home-swiper :banners="banners"/>
      <recommend-view :recommends="recommends"/>
      <feature-view/>
      <tab-control class="tab-control"
                  :titles="['流行','新款','精选']"
                  @tabClick="tabClick"/>
    <goods-list :goods="showGoods"/>
    </scroll>
    <back-top @click.native="backClick" v-show="isShowBackTop"/>
  </div>
</template>

<script>
import HomeSwiper from './childComps/HomeSwiper'
import RecommendView from './childComps/RecommendView'
import FeatureView from './childComps/FeatureView.vue'

import NavBar from 'components/common/navbar/NavBar'
import TabControl from 'components/content/tabControl/tabControl.vue'
import Scroll from 'components/common/scroll/Scroll.vue'
import GoodsList from 'components/content/goods/GoodsList.vue'
import BackTop from 'components/content/backTop/BackTop.vue'

import {getHomeMultidata,getHomeGoods} from "network/home"



  export default {
    name: "Home",
    components:{
      HomeSwiper,
      RecommendView,
      FeatureView,
      NavBar,
      TabControl,
      Scroll,
      GoodsList,
      BackTop,

      // Swiper,
      // SwiperItem,
    },
    data (){
        return{
        banners:[], // []并不表示数组，只是表示一个初始化值
        recommends:[],
        goods:{
          'pop':{page:0,list:[]},
          'new':{page:0,list:[]},
          'sell':{page:0,list:[]},
        },
        currentType:'pop',
        isShowBackTop:false
      }
    },
    computed:{
      showGoods(){
        return this.goods[this.currentType].list
      }
    },
    created(){  /*生命周期函数 */
      this.getHomeMultidata();  //因为设置的函数名一样，调用下面函数要加this
      this.getHomeGoods('pop')  //否则调用的是引用的函数
      this.getHomeGoods('new')
      this.getHomeGoods('sell')
    },
    methods:{
      /*时间监听相关方法 */
      tabClick(index){
        // console.log(index);
      this.currentType = Object.keys(this.goods)[index]
      },
      backClick(){
        this.$refs.scroll.scrollTo(0,0,500)
      },
      contentScroll(position){
        this.isShowBackTop = (-position.y) > 1000
      },
      loadMore(){
        this.getHomeGoods(this.currentType)
        this.$refs.scroll.scroll.refresh()
      },
      /*网络请求相关方法*/
      getHomeMultidata(){   //多封装一层
        getHomeMultidata().then(res =>{
        // console.log(res.data);
        this.banners = res.data.banner.list;   //箭头函数中的this指向上下文，此处this指向vue实例
        this.recommends = res.data.recommend.list;
      })
      },
      getHomeGoods(type) {
        const page = this.goods[type].page + 1 //当组件创建完即显示第一页的数据
        getHomeGoods(type,page).then(res =>{
          this.goods[type].list.push(...res.data.list) //res.data.list为获取到的数据
          this.goods[type].page +=1  //当执行完上面一行代码有新数据push后页码加一
          this.$refs.scroll.finishPullUp()
        })
      }

    }
  }
</script>

<style scoped>
  #home {
    padding-top: 44px;
    height: 100vh; /*vh 视口高度 100vh表示100%的视口*/

    position: relative;
  }
  .tab-control {
    position: sticky;
    top: 44px;
    z-index: 9;
  }

   .home-nav {
    background-color: var(--color-tint);
    color: #fff;
    position: fixed;
    left: 0;
    right: 0;
    top: 0;
    z-index:9;
  }
  .content {
    overflow: hidden;
    position: absolute; /*利用定位设置滚动区域 */
    top: 44px;
    bottom: 49px;
    left: 0;
    right: 0;
  }
</style>
