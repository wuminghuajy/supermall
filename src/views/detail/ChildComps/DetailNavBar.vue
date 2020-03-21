<template>
    
      <nav-bar>
        <div slot="left" class="back" @click="backClick">
          <img src="~assets/img/common/back.svg" alt="">
        </div>
        <div slot="center" class="title">
          <div v-for="(item,index) in titles" 
              class="title-item"
              :class="{active:index === currentindex}"
              @click="titleClick(index)">
              {{item}}
          </div>
        </div>
      </nav-bar>
     
</template>

<script>
import NavBar from 'components/common/navbar/NavBar'

export default {
    name:'DetailNavBar',
    components:{
      NavBar
    },
    data() {
        return {
          titles:['商品','参数','评论','推荐'],
          currentindex:0
        };
    },
    created() {

    },
    mounted() {

    },
    methods: {
      titleClick(index){
        this.currentindex = index
      },
      backClick(){
        this.$router.go(-1)
      }
    }
};
</script>

<style scoped >

.title{
  display: flex;
}
.title-item{
  flex: 1;
  font-size: 13px;
}
.active{
  color: var(--color-high-text);
}
.back{
  margin-top: 6px;
}
</style>
