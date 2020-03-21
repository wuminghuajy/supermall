<template>
    <div id="detail">
    <detail-nav-bar class="detail-nav"></detail-nav-bar>
    <scroll class="content" ref="scroll">
      <detail-swiper :top-images="topImages"/>
      <detail-base-info :goods="goods"/>
      <detail-shop-info :shop="shop"/>
      <detail-goods-info :detail-info = "detailInfo"/>
    </scroll>
    
    </div>
</template>

<script>
import DetailNavBar from './ChildComps/DetailNavBar'
import DetailSwiper from './ChildComps/DetailSwiper'
import DetailBaseInfo from './ChildComps/DetailBaseInfo'
import DetailShopInfo from './ChildComps/DetailShopInfo'
import DetailGoodsInfo from './ChildComps/DetailGoodsInfo'
import {getDetail, Goods,Shop} from  'network/detail'
import Scroll from 'components/common/scroll/Scroll'

export default {
    name:'Detail',
    data() {
        return {
          iid:null,
          topImages: [],
          goods: {},
          shop:{},
          detailInfo:{}
        };
    },
    components:{
      DetailNavBar,
      DetailSwiper,
      DetailBaseInfo,
      DetailShopInfo,
       DetailGoodsInfo,
      Scroll
    },
    created() {
      //1.保存传入的iid
      this.iid = this.$route.params.iid

      //2.根据iid请求详情数据
      getDetail(this.iid).then(res => {
        // console.log(res);
        const data = res.result;
        //1.获取顶部轮播数据
        this.topImages = data.itemInfo.topImages
        //2.获取商品信息
         this.goods = new Goods(data.itemInfo, data.columns, data.shopInfo.services)
         //3.获取商家信息
         this.shop = new Shop(data.shopInfo)
         //4.保存商品的详情数据
         this.detailInfo = data.detailInfo
      })
    },
    mounted() {

    },
    methods: {

    }
};
</script>

<style scoped >
 #detail {
    position: relative;
    z-index: 9;
    background-color: #fff;
    height: 100vh;
  }

  .detail-nav {
    position: relative;
    z-index: 9;
    background-color: #fff;
  }

  .content {
    height: calc(100% - 44px);
  }
</style>
