<template>
  <div>
    <nav-bar>
      <div slot="left" class="back" @click="backClick">
        <img src="~assets/img/common/back.svg" alt="">
      </div>
      <div slot="center" class="title">
        <div v-for="(item,index) in titles" :key="index" class="titleItem" :class="{active: index === currentIndex}" @click="titleClick(index)">
          {{item}}
        </div>
      </div>
    </nav-bar>
  </div>
</template>

<script>
  import NavBar from 'components/common/navBar/NavBar'

  export default {
    name:'',
    props:[''],
    data () {
      return {
        titles: ['商品', '参数', '评论', '推荐'],
        currentIndex: 0
      };
    },

    components: {
      NavBar
    },

    methods: {
      titleClick(index){
        this.currentIndex = index
      },
      backClick() {
        this.$router.back()
      }
    },

    watch: {}

  }

</script>
<style scoped>
.title {
  display: flex;
}
.titleItem {
  flex: 1;
}
.active {
  color: var(--color-high-text);
}
.back img{
  margin-top: 10px;
}
</style>