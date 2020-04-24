<template>
    <div id="detail">
        <detail-nav-bar class="detail-nav"></detail-nav-bar>
        <scroll class="content" ref="scroll">
            <detail-swiper :top-images="topImages"/>
            <detail-base-info :goods="goods"/>
            <detail-shop-info :shop="shop"/>
            <detail-goods-info :detail-info="detailInfo" @imageLoad="imageLoad"/>
            <detail-param-info :param-info="paramInfo"></detail-param-info>
            <detail-comment-info :comment-info="commentInfo"/>
        </scroll>
    </div>
</template>

<script>
    import DetailNavBar from './childcomponent/DetailNavBar'
    import DetailSwiper from './childcomponent/DetailSwiper'
    import DetailBaseInfo from './childcomponent/DetailBaseInfo'
    import DetailShopInfo from './childcomponent/DetailShopInfo'
    import DetailGoodsInfo from './childcomponent/DetailGoodsInfo'
    import DetailParamInfo from './childcomponent/DetailParamInfo'
    import DetailCommentInfo from './childcomponent/DetailCommentInfo'

    import Scroll from 'components/common/scroll/Scroll'

    import {getDetail,Goods,Shop,GoodsParam} from "network/detail";

    export default {
        name: "Detail",
        components:{
            DetailNavBar,
            DetailSwiper,
            DetailBaseInfo,
            DetailShopInfo,
            DetailGoodsInfo,
            DetailParamInfo,
            DetailCommentInfo,
            Scroll
        },
        data(){
            return {
                iid: null,
                topImages: [],
                goods: {},
                shop: {},
                detailInfo:{},
                paramInfo:{},
                commentInfo:{}
            }
        },
        methods:{
            imageLoad(){
             this.$refs.scroll.refresh()
            }
        },
        created() {
            this.iid = this.$route.params.iid

            getDetail(this.iid).then(res =>{
                console.log(res)
                const data = res.result
                this.topImages = data.itemInfo.topImages
                 //获取商品信息
                this.goods = new Goods(data.itemInfo,data.columns,data.shopInfo.services)
                 //创建店铺信息
                this.shop = new Shop(data.shopInfo)

                //保存商品的详情数据
                this.detailInfo = data.detailInfo;
                //取出参数信息
                //this.paramInfo = data.itemParams
                //console.log(data.itemParams)
                // 5.获取参数的信息
                this.paramInfo = new GoodsParam(data.itemParams.info, data.itemParams.rule)
                //6.获商品的评论信息
                if(data.rate.cRate !==0){
                    this.commentInfo = data.rate.list[0]
                    console.log(this.commentInfo)
                }
            })

        }
    }
</script>

<style scoped>
    #detail{
        height: 100vh;
        position: relative;
        z-index: 9;
        background-color: #fff;
    }
    .detail-nav {
        position: relative;
        background-color: #fff;
        z-index: 9;
    }
    .content{
        height: calc(100% - 44px);
    }

</style>