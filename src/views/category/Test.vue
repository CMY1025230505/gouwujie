<template>
  <div class="wrapper" ref="wrapper">
    <slot></slot>
  </div>
</template>

<script>
import BScroll from "better-scroll";

export default {
  name: "",
  components: {
  },
  mounted() {
    this.scroll = new BScroll(this.$refs.wrapper, {
    });
    console.log(this.scroll);
  },
  data() {
    return {
      scroll: null,
    };
  },
  methods: {
    click() {
      console.log(1);
    },
  },
};
</script>
<style scoped>
.wrapper {
  height: 150px;
  background-color: aqua;
  overflow: hidden;
}
</style>