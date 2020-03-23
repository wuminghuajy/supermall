<template>
    <div id="detail">
    <detail-nav-bar ref="nav" class="detail-nav" @titleClick="titleClick"></detail-nav-bar>
    <scroll class="content" ref="scroll" :probe-type = '3' @scroll="contentScroll">
      <detail-swiper :top-images="topImages"/>
      <detail-base-info :goods="goods"/>
      <detail-shop-info :shop="shop"/>
      <detail-goods-info :detail-info = "detailInfo" @imgLoad="imgLoad" ref="goodsInfo"/>
      <detail-params-info :param-info="paramInfo" ref="params"/>
      <detail-comment-info :comment-info = 'commentInfo' ref="comment"/>
      <goods-list :goods="recommend" ref="recommend"/>
    </scroll>
    
    </div>
</template>

<script>
import DetailNavBar from './ChildComps/DetailNavBar'
import DetailSwiper from './ChildComps/DetailSwiper'
import DetailBaseInfo from './ChildComps/DetailBaseInfo'
import DetailShopInfo from './ChildComps/DetailShopInfo'
import DetailGoodsInfo from './ChildComps/DetailGoodsInfo'
import DetailParamsInfo from './ChildComps/DetailParamsInfo'
import DetailCommentInfo from './ChildComps/DetailCommentInfo'
import {getDetail, Goods,Shop,GoodsParam,getRecommend} from  'network/detail'
import {debounce} from 'common/utils'
import Scroll from 'components/common/scroll/Scroll'
import GoodsList from "components/content/goods/GoodsList"

export default {
    name:'Detail',
    data() {
        return {
          iid:null,
          topImages: [],
          goods: {},
          shop:{},
          detailInfo:{},
          paramInfo:{},
          commentInfo:{},
          recommend:[],
          themeTopYs:[],
          getThemeTopY :null,
          currentindex:0
        };
    },
    components:{
      DetailNavBar,
      DetailSwiper,
      DetailBaseInfo,
      DetailShopInfo,
      DetailGoodsInfo,
      DetailParamsInfo,
      DetailCommentInfo,
      Scroll,
      GoodsList
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
         this.detailInfo = data.detailInfo;
         //5.获取参数信息
         this.paramInfo = new GoodsParam(data.itemParams.info,data.itemParams.rule)
         //6.取出评论信息
         if(data.rate.cRate !== 0){
           this.commentInfo = data.rate.list[0]
         }    

        
      }),
      //3.获取推荐数据
      getRecommend().then(res => {
        this.recommend = res.data.list
      }),

      //4.给getThemeTopY赋值
      this.getThemeTopY = debounce(() =>{
        this.themeTopYs = []
        this.themeTopYs.push(0);
        this.themeTopYs.push(this.$refs.params.$el.offsetTop);
        this.themeTopYs.push(this.$refs.comment.$el.offsetTop);
        this.themeTopYs.push(this.$refs.recommend.$el.offsetTop);
        // this.themeTopYs.push(Number.MAX_VALUE)
        // console.log(this.themeTopYs);
      })
        // console.log(this.themeTopYs);
    },
   
    methods: {
    imgLoad(){
        this.$refs.scroll.refresh()
        this.getThemeTopY()
      },
      titleClick(index){
        // console.log(index);
        this.$refs.scroll.scrollTo(0,-this.themeTopYs[index],500)
      },
      contentScroll(position){
        // console.log(position);
        //1.获取y值
        const positionY = -position.y
        // 2.positionY 与 主题中值进行对比
        let length = this.themeTopYs.length;
        for(let i =0; i < length; i++) {
          if(this.currentindex != i && ((i < length - 1 && positionY >=  this.themeTopYs[i] 
          && positionY < this.themeTopYs[i+1]) || (i === length - 1 && positionY >= this.themeTopYs[i]))){
            this.currentindex = i;
            console.log(this.currentindex);
            
            this.$refs.nav.currentindex = this.currentindex
          }
        };
      }
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
    overflow: hidden;
    position: absolute;
    top:44px;
  }

  .goods-info{
    position: relative;
  }
</style>
