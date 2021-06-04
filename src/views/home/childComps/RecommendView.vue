<template>
  <div class="recommend">
    <div v-for="item in recommends" class="recommend-item">
      <a :herf="item.link">
        <img :src="item.image" alt="">
        <div>{{item.title}}</div>
      </a>
    </div>
  </div>
</template>

<script>
export default {
  name: 'RecommendView',
  props:{
    recommends:{
      type:Array,
      dafault(){
        return[]
      }
    }
  },

}

</script>

<style scoped>
  .recommend {
    display: flex;
    text-align: center;
    font-size: 12px;
    padding: 10px 0 20px;
    border-bottom: 8px solid #eee;
  }
  .recommend-item {
    flex: 1;
  }
  .recommend-item img {
    width: 90%;
    margin-bottom: 10px;
  }
</style>
