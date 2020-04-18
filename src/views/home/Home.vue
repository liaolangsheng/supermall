<template>
  <div id="home">
    <nav-bar class="home-nav-bar"><div slot="center">购物街</div></nav-bar>
    <home-swiper :banners="banners"></home-swiper>
    <recommend-view :recommends="recommends"></recommend-view>
    <feature-view></feature-view>
    <div>这是扩展内容</div>
    <div>这是扩展内容</div>
    <div>这是扩展内容</div>
    <div>这是扩展内容</div>
    <div>这是扩展内容</div>
    <div>这是扩展内容</div>
  </div>
</template>

<script>
  import NavBar from "components/common/navbar/NavBar"
  import HomeSwiper from "./childComponent/HomeSwiper"
  import RecommendView from "./childComponent/RecommendView"
  import FeatureView from "./childComponent/FeatureView"
  import {getMultiData} from "network/home"

  export default {
    name: "Home",
    components:{
      NavBar,
      HomeSwiper,
      RecommendView,
      FeatureView
    },
    data(){
      return {
        banners: [],
        recommends:[]
      }
    },
    created() {
      getMultiData().then(res => {
        this.banners = res.data.banner.list;
        this.recommends = res.data.recommend.list;
      })
    }
  }
</script>

<style scoped>
  #home{
    padding-top: 44px;
  }
  .home-nav-bar{
    background-color: var(--color-tint);
    color: #fff;
  }

</style>
