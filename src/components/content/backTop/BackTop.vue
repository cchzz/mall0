<template>
  <div class="back-top" >
    <img src="~assets/img/common/top.png" alt="" srcset="">
  </div>
</template>

<script>
export default {
  name: 'BackTop',
}

</script>

<style scoped>
  .back-top {  /*因为每个页面都需要这个功能，所以样式直接在组件中设置 */
    position: fixed;
    right: 8px;
    bottom: 55px;
  }
  .back-top img {
    width: 43px;
    height: 43px;
  }
</style>
