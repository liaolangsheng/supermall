<template>
    <nav-bar>
        <div slot="left">
            <img src="~assets/img/common/back.svg" alt="" @click="backClick">
        </div>
        <div slot="center" class="title">
            <div v-for="(item,index) in titles"
                 class="title-item"
                 :class="{active: currentIndex === index}"
                 @click="itemClick(index)">
                {{item}}
            </div>
        </div>
    </nav-bar>
</template>

<script>
    import NavBar from "components/common/navbar/NavBar";
    export default {
        name: "DetailNavBar",
        components:{
            NavBar
        },
        data(){
            return {
                titles: ['商品','参数','评论','推荐'],
                currentIndex: 0
            }
        },
        methods:{
            itemClick(index){
                this.currentIndex = index
                console.log(index);
            },
            backClick(){
                this.$router.back()
            }
        }
    }
</script>

<style scoped>
    .title {
        display: flex;
        font-size: 13px;
    }
    .title-item {
        flex: 1;

    }
    .active {
        color: var(--color-high-text);
    }
    .left img {
        margin-top: 10px;
    }

</style>