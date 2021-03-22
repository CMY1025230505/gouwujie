<template>
    <swiper class="swiperItem">
      <swiper-item v-for="(item,index) in topImage" :key="index">
        <img :src="item" alt="">
      </swiper-item>
    </swiper>
</template>

<script>
  import {Swiper, SwiperItem} from 'components/common/swiper'
  export default {
    name:'',
    props:{
      topImage: {
        type: Array,
        default() {
          return []
        }
      }
    },
    data () {
      return {

      };
    },

    components: {
      Swiper,
      SwiperItem
    },

  }

</script>
<style scoped>
.swiperItem {
  height: 300px;
  overflow: hidden;
}
</style>