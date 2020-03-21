<template>
    <swiper class="detail-swiper">
    <swiper-item v-for="item in topImages"
    class="swipe-item">
      <img :src="item"  alt="">
    </swiper-item>
  </swiper>
</template>

<script>
import {Swiper,SwiperItem} from 'components/common/swiper'
 export default {
    name: "DetailSwiper",
    props: {
      topImages: {
        type: Array,
        default() {
          return []
        }
      }
    },
    components: {
      Swiper,
      SwiperItem
    }
    
  }
</script>

<style scoped >
.swiper{
  height: 300px;
}
.swipe-item{
  height: 300px;
  overflow: hidden;
}
</style>
