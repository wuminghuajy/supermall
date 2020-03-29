<template>
    <div class="cart-list">
      <scroll class="content" ref="srcoll">
      <cart-list-item 
      v-for="(item,index) in cartList" 
      :product="item"
      :key="index"/>
      </scroll>
      <cart-bottom-bar/>
    </div>
</template>

<script>
import Scroll from 'components/common/scroll/Scroll'
import CartListItem from './CartListItem'
import CartBottomBar from './CartBottomBar'
import {mapGetters} from 'vuex'
export default {
    name:'CartList',
    components: {
      Scroll,
      CartListItem,
      CartBottomBar
    },
    computed:{
      ...mapGetters(
        ['cartList']
        )
    },
    activated(){
      this.$refs.srcoll.refresh()
    }
};
</script>

<style scoped >
.cart-list {
    height: calc(100% - 44px); 
  }

  .content {
    height: calc(100% - 95px);
    overflow: hidden;
  }
</style>
