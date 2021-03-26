<template>
  <div class="cart">
    <nav-bar>
      <div slot="center" class="nav-bar">购物车({{$store.state.cartList.length}})</div>
    </nav-bar>

    <cart-list></cart-list>
  </div>
</template>

<script>
  import NavBar from 'components/common/navBar/NavBar'
  import CartList from './childComps/CartList'

  export default {
    name:'Cart',
    props:[''],
    data () {
      return {

      };
    },

    components: {
      NavBar,
      CartList
    },

    computed: {},

    beforeMount() {},

    mounted() {},

    methods: {},

    watch: {}

  }

</script>
<style scoped>
.nav-bar {
  background-color: var(--color-tint);
  color: #fff;
  font-weight: 400;
}
</style>