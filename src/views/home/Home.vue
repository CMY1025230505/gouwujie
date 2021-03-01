<template>
  <div id="home">
    <nav-bar class="home-nav">
      <div slot="center">购物街</div>
    </nav-bar>
  </div>
</template>

<script>
  import NavBar from 'components/common/navBar/NavBar'

  export default {
    name:'Home',
    components: {
      NavBar,
      HomeSwiper
    },
    created() {
      getHomeMultidata().then(res => {
        this.banners = res.data.banner.list;
        this.recommends = res.data.recommend.list
      })
    },
    data() {
      return {
        banners: [],
        recommends: [],
      }
    },
  }

</script>
<style scoped>
  .home-nav {
    background-color: var(--color-tint);
    color: white;
  }
</style>