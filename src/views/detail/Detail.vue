<template>
  <div id="detail">
    <detail-nav-bar
      class="detail-nav"
      @titleClick="titleClick"
    ></detail-nav-bar>
    <scroll class="content" ref="scroll">
      <detail-swiper :topImage="topImage"></detail-swiper>
      <detail-base-info :goods="goods"></detail-base-info>
      <detail-shop-info :shop="shop"></detail-shop-info>
      <detail-goods-info
        :detailInfo="detailInfo"
        @imgLoad="imgLoad"
      ></detail-goods-info>
      <detail-param-info :param-info="paramInfo" ref="params"></detail-param-info>
      <goods-list :goods="recommend" ref="recommend"></goods-list>
    </scroll>
    <detail-bottom-bar @addCart="addCart"></detail-bottom-bar>
  </div>
</template>

<script>
import DetailNavBar from "./childComps/DetailNavBar";
import DetailBottomBar from "./childComps/DetailBottomBar";
import DetailSwiper from "./childComps/DetailSwiper";
import DetailBaseInfo from "./childComps/DetailBaseInfo";
import DetailShopInfo from "./childComps/DetailShopInfo";
import DetailGoodsInfo from "./childComps/DetailGoodsInfo";
import DetailParamInfo from "./childComps/DetailParamInfo";

import Scroll from "components/common/scroll/Scroll";
import GoodsList from "components/content/goods/GoodsList";

import {
  getDetail,
  Goods,
  Shop,
  GoodsParam,
  getRecommend,
} from "network/detail";

export default {
  name: "Detail",
  props: [""],
  data() {
    return {
      iid: null,
      topImage: null,
      goods: {},
      shop: {},
      detailInfo: {},
      paramInfo: {},
      recommend: [],
      // offsetTOPY: []
    };
  },
  methods: {
    imgLoad() {
      this.$refs.scroll.refresh();
    },
    debounce(func, delay) {
      let timer = null;

      return function (...args) {
        if (timer) {
          clearTimeout(timer);
        }
        timer = setTimeout(() => {
          func.apply(this, args);
          console.log(2);
        }, delay);
      };
    },
    titleClick(index) {
      console.log(index);
      // this.$refs.scroll.scroll.scrollTo(0,-this.offsetTOPY[index],500)
    },
    addCart() {
      let product = {}
      product.image = this.topImage[0]
      product.title = this.goods.title
      product.desc = this.goods.desc
      product.price = this.goods.newPrice
      product.iid = this.iid

      this.$store.commit('addCart',product)
      console.log(this.detailInfo);
    }
  },
  created() {
    this.iid = this.$route.params.iid;

    getDetail(this.iid).then((res) => {
      const data = res.result;
      console.log(data);
      this.topImage = data.itemInfo.topImages;

      this.goods = new Goods(
        data.itemInfo,
        data.columns,
        data.shopInfo.services
      );
      this.shop = new Shop(data.shopInfo);
      this.detailInfo = data.detailInfo;
      this.paramInfo = new GoodsParam(
        data.itemParams.info,
        data.itemParams.rule
      );

      getRecommend().then((res) => {
        console.log(res);
        this.recommend = res.data.list;
      });
    });
  },
  mounted() {
    const refresh = this.debounce(this.$refs.scroll.refresh, 500);
    this.$bus.$on("detailItemImageLoad", () => {
      refresh();
      console.log(1);
      /* this.debounce(() => {
        this.offsetTOPY.push(0)
        this.offsetTOPY.push(this.$refs.params.$el.offsetTop)
        this.offsetTOPY.push(this.$refs.params.$el.offsetTop)
        this.offsetTOPY.push(this.$refs.recommend.$el.offsetTop)

        console.log(this.offsetTOPY);
      },300)() */
    });
  },
  components: {
    DetailNavBar,
    DetailSwiper,
    DetailBaseInfo,
    DetailShopInfo,
    Scroll,
    DetailGoodsInfo,
    DetailParamInfo,
    GoodsList,
    DetailBottomBar,
  },
};
</script>
<style scoped>
#detail {
  position: relative;
  z-index: 9;
  background-color: #fff;
  height: 100vh;
}
.content {
  /* position: absolute;
  top: 44px;
  bottom: 49px;
  left: 0;
  right: 0; */
  position: relative;
  height: calc(100vh - 44px - 49px);
  overflow: hidden;
}
.detail-nav {
  position: relative;
  z-index: 9;
  background-color: #fff;
}
</style>