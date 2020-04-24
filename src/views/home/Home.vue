<template>
  <div id="home" class="wrapper">
    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>
    <tab-control :titles="['流行','新款','精选']"
                 class="tab-control"
                 @tabClick="tabclick" ref="tabControl1" v-show="isTabFix"/>
    <scroll class="content"
            ref="scroll"
            :probe-type="3"
            :pull-up-load="true"
            @scroll="scrolltop"
            @pullingUp="loadMore">
      <home-swiper :banners="banners" @swiperImageLoad="swiperImageLoad"/>
      <recommend-view :recommends="recommends"/>
      <feature-view/>
      <tab-control :titles="['流行','新款','精选']"
                   @tabClick="tabclick"
                    ref="tabControl2" />
      <goods-list :goods="goods[currentType].list"/>
    </scroll>
    <back-top  @click.native="backClick" v-show="isShowBackTop"/>

  </div>
</template>

<script>
  import HomeSwiper from "./childComponent/HomeSwiper"
  import RecommendView from "./childComponent/RecommendView"
  import FeatureView from "./childComponent/FeatureView"

  import NavBar from "components/common/navbar/NavBar"
  import TabControl from "components/content/tabControl/TabControl"
  import GoodsList from "components/content/goods/GoodsList"
  import Scroll from "components/common/scroll/Scroll"
  import BackTop from "components/content/backTop/BackTop"

  import {getHomeMultiData, getHomeGoodsData} from "network/home"

  export default {
    name: "Home",
    components:{
      HomeSwiper,
      RecommendView,
      FeatureView,
      NavBar,
      TabControl,
      GoodsList,
      Scroll,
      BackTop
    },
    data(){
      return {
        banners: [],
        recommends:[],
        goods:{
          'pop': {page:0, list:[] },
          'new': {page:0, list:[] },
          'sell': {page:0, list:[] }
        },
        currentType: 'pop',
        isShowBackTop:false,
        tabOffsetTop: 0,
        isTabFix: false,
        saveY: 0
      }
    },
    activated(){
      this.$refs.scroll.refresh()
      this.$refs.scroll.scrollTo(0,this.saveY,0)
    },
    deactivated(){
      this.saveY = this.$refs.scroll.getScrollY()
    },
    created() {
      //请求多个数据
      this.getHomeMultiData()
      //请求商品数据
      this.getHomeGoodsData('pop')
      this.getHomeGoodsData('new')
      this.getHomeGoodsData('sell')
    },
    mounted(){

      //监听item中图片加载完成
      this.$bus.$on('itemImageLoad',() => {
        this.$refs.scroll.refresh()
      })
    },

    methods:{
      tabclick(index){
        switch (index) {
          case 0:
            this.currentType = 'pop'
                break;
          case 1:
            this.currentType ='new'
                break;
          case 2:
            this.currentType = 'sell'
        }
        this.$refs.tabControl1.currentIndex = index;
        this.$refs.tabControl2.currentIndex = index;
      },
      backClick(){
        this.$refs.scroll.scroll.scrollTo(0,0,500)
      },

      scrolltop(position){
        //显示回到顶部图标
        this.isShowBackTop =( -position.y) >1000
        //tabControl吸顶
        this.isTabFix =( -position.y) > this.tabOffsetTop
      },

      loadMore(){
        this.getHomeGoodsData(this.currentType)
      },
      swiperImageLoad(){
       this.tabOffsetTop = this.$refs.tabControl2.$el.offsetTop
      },

      /**
       * 网络请求模块
       */
      getHomeMultiData(){
        getHomeMultiData().then(res => {
          this.banners = res.data.banner.list;
          this.recommends = res.data.recommend.list;
        })
      },
      getHomeGoodsData(type){
        const page = this.goods[type].page +1;
        getHomeGoodsData(type,page).then(res => {
          this.goods[type].list.push(...res.data.list);
          this.goods[type].page +=1

          //完成上拉加载更多
          this.$refs.scroll.finishPullUp()
        })
      },
    }
  }
</script>

<style scoped>
  #home{
    padding-top: 44px;
    height: 100vh;
    position: relative;
  }
  .home-nav{
    background-color: var(--color-tint);
    color: #fff;

    position: fixed;
    left: 0;
    right: 0;
    top: 0;
    z-index: 9;
  }
  .content {
    overflow: hidden;
    position: absolute;
    top: 44px;
    bottom: 49px;
    left: 0;
    right: 0;
  }

  .tab-control {
    background-color: #fff;
    position: relative;
    z-index: 9;
    /*这里是为了在浏览器原生滚动时，让导航不随着一起滚动*/
   /* position: sticky;
    top: 44px;
    left: 0;
    right: 0;
    z-index: 9;*/
  }


</style>
