<template>
  <div id="detail">
    <detail-nav-bar></detail-nav-bar>
    <detail-swiper :topImage="topImage"></detail-swiper>
  </div>
</template>

<script>
  import DetailNavBar from './childComps/DetailNavBar'
  import DetailSwiper from './childComps/DetailSwiper'
  import { getDetail } from "network/detail";

  export default {
    name:'Detail',
    props:[''],
    data () {
      return {
        iid: null,
        topImage: null
      };
    },
    created() {
      this.iid = this.$route.params.iid

      getDetail(this.iid).then(res => {
        this.topImage = res.result.itemInfo.topImages
      })
    },
    components: {
      DetailNavBar,
      DetailSwiper
    }
  }

</script>
<style lang='' scoped>

</style>