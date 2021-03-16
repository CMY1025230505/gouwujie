<template>
  <div ref="wrapper">
    <div>
      <slot></slot>
    </div>
  </div>
</template>
<script>
import BScroll from "better-scroll";

export default {
  name: "Scroll",
  mounted() {
    this.scroll = new BScroll(this.$refs.wrapper, {
      probeType: 3,
      click: true,
      pullUpLoad: true,
    });

    this.scroll.on('scroll',(position) => {
      this.$emit('scroll',position)
    })

    this.scroll.on('pullingUp',(position) => {
      this.$emit('pullingUp',position)
    })
  },
  data() {
    return {
      scroll: null,
    };
  },
  computed: {
  }
};
</script>
<style scoped>
</style>