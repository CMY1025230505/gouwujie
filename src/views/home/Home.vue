<template>
  <div id="home">
    <nav-bar class="home-nav">
      <div slot="center">购物街</div>
    </nav-bar>
    <tab-control
      ref="tabControl1"
      :titles="['流行', '新款', '精选']"
      @tabClick="tabClick"
      class="tabCopy"
      v-show="isTabFixed"
    />
    <scroll
      class="content"
      ref="scroll"
      @scroll="contentScroll"
      @pullingUp="loadMore"
    >
      <home-swiper :banners="banners" @swiperImageLoad="swiperImageLoad" />
      <recommend-view :recommends="recommends" />
      <feature-view />
      <tab-control
        ref="tabControl2"
        :titles="['流行', '新款', '精选']"
        @tabClick="tabClick"
      />
      <goods-list :goods="showGoods" />
    </scroll>
    <backTop @click.native="btClick" v-show="backTopShow" />
  </div>
</template>

<script>
import HomeSwiper from "./childComps/HomeSwiper";
import RecommendView from "./childComps/RecommendView";
import FeatureView from "./childComps/FeatureView";

import NavBar from "components/common/navBar/NavBar";
import TabControl from "components/content/tabControl/TabControl";
import GoodsList from "components/content/goods/GoodsList";
import Scroll from "components/common/scroll/Scroll";
import BackTop from "components/content/backTop/BackTop";

import { getHomeMultidata, getHomeGoods } from "network/home";

export default {
  name: "Home",
  components: {
    NavBar,
    HomeSwiper,
    RecommendView,
    FeatureView,
    TabControl,
    GoodsList,
    Scroll,
    BackTop,
  },
  created() {
    this.getHomeMultidata();
    this.getHomeGoods("pop");
    this.getHomeGoods("new");
    this.getHomeGoods("sell");
  },
  mounted() {
    const refresh = this.debounce(this.$refs.scroll.refresh, 500);
    this.$bus.$on("homeItemImageLoad", () => {
      refresh();
    });
    console.log(this.$refs.scroll.scroll);
  },
  activated() {
    this.$refs.scroll.refresh()
    this.$refs.scroll.scroll.scrollTo(0, this.saveY, 0);
  },
  deactivated() {
    console.log(this.$refs.scroll.scroll.y);
    this.saveY = this.$refs.scroll.scroll.y;
  },
  data() {
    return {
      banners: [],
      recommends: [],
      goods: {
        pop: { page: 0, list: [] },
        new: { page: 0, list: [] },
        sell: { page: 0, list: [] },
      },
      currentType: "pop",
      backTopShow: false,
      tabOffsetTop: 0,
      isTabFixed: false,
      saveY: 0,
    };
  },
  computed: {
    showGoods() {
      return this.goods[this.currentType].list;
    },
  },
  methods: {
    tabClick(index) {
      switch (index) {
        case 0:
          this.currentType = "pop";
          break;
        case 1:
          this.currentType = "new";
          break;
        case 2:
          this.currentType = "sell";
          break;
      }
      this.$refs.tabControl1.currentIndex = index;
      this.$refs.tabControl2.currentIndex = index;
    },
    btClick() {
      // this.$refs.scroll.scroll.refresh();
      this.$refs.scroll.scroll.scrollTo(0, 0, 500);
    },
    contentScroll(position) {
      this.backTopShow = position.y + 750 < 0;

      this.isTabFixed = -position.y > this.tabOffsetTop;
    },
    loadMore() {
      console.log("yes");
      this.getHomeGoods(this.currentType);
      this.$refs.scroll.refresh();
    },
    swiperImageLoad() {
      this.tabOffsetTop = this.$refs.tabControl2.$el.offsetTop;
    },
    debounce(func, delay) {
      let timer = null;

      return function (...args) {
        if (timer) {
          clearTimeout(timer);
        }
        timer = setTimeout(() => {
          func.apply(this, args);
        }, delay);
      };
    },

    // 网络请求
    getHomeMultidata() {
      getHomeMultidata().then((res) => {
        this.banners = res.data.banner.list;
        this.recommends = res.data.recommend.list;
      });
    },
    getHomeGoods(type) {
      let page = this.goods[type].page + 1;
      getHomeGoods(type, page).then((res) => {
        this.goods[type].list.push(...res.data.list);
        this.goods[type].page += 1;

        this.$refs.scroll.scroll.finishPullUp();
      });
    },
  },
};
</script>
<style scoped>
#home {
  padding-top: 44px;
  /* height: calc(100vh - 49px); */
  height: 100vh;
  position: relative;
}
.home-nav {
  background-color: var(--color-tint);
  color: white;

  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1;
}
.content {
  /* position: absolute;
  top: 44px;
  height: calc(100% - 44px); */
  position: absolute;
  top: 44px;
  bottom: 49px;
  left: 0;
  right: 0;
  overflow: hidden;
}
.tabCopy {
  position: relative;
  z-index: 3;
  top: 0;
}
</style>