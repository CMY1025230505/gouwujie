<template>
  <div>
    <cart-list-item v-for="(item,index) in $store.state.cartList" :key="index" :product="item"></cart-list-item>
  </div>
</template>

<script>
  import CartListItem from './CartListItem'
  export default {
    name:'',
    props:[''],
    data () {
      return {

      };
    },

    components: {
      CartListItem
    },

    computed: {},

    beforeMount() {},

    mounted() {},

    methods: {},

    watch: {}

  }

</script>
<style lang='' scoped>

</style>