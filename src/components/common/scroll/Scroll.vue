<template>
    <div class="scroll" ref="swiper">
        <div class="content">
            <slot></slot>
        </div>
    </div>
</template>

<script>
    import Bscroll from 'better-scroll'
    export default {
        name: "Scroll",
        props:{
            probeType:{
                type: Number,
                default:0
            },
            pullUpLoad:{
                type:Boolean,
                default: false
            }
        },
        data(){
            return {
                scroll: null
            }
        },
        mounted() {
            //创建Bscroll对象
            this.scroll = new Bscroll(this.$refs.swiper,{
                click:true,
                probeType: this.probeType,
                pullUpLoad: this.pullUpLoad
            })
            //监听滚动事件
            this.scroll.on('scroll',(position) =>{
               this.$emit('scroll',position)
            })
            //监听滚动到底部
            this.scroll.on('pullingUp',() =>{
               this.$emit('pullingUp')
            })

            //上拉加载更多

        },
        methods:{
            scrollTo(x,y,time){
                this.scroll && this.scroll.scrollTo(x,y,time)
            },
            refresh(){
                this.scroll && this.scroll.refresh()
            },
            finishPullUp(){
                this.scroll.finishPullUp()
            },
            getScrollY(){
               return  this.scroll ? this.scroll.y : 0
            }

        }
    }
</script>

<style scoped>

</style>