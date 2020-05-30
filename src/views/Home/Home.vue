<template>
    <div class="position-box">


        <vue-better-scroll class="wrapper"
                           ref="scroll"
                           :scrollbar="scrollbarObj"
                           :pullDownRefresh="pullDownRefreshObj"
                           :pullUpLoad="pullUpLoadObj"
                           :startY="parseInt(startY)"
                           @pulling-down="onPullingDown"
                           @pulling-up="onPullingUp">
            <div class="goods-list">

                <div class="goods-item" v-for="item in goodsList" :key="item.id">
                    <img :src="item.thumbnail">
                    <h1 class="title">{{ item.goodsName }}</h1>
                    <div class="info">
                        <p class="price">
                            <span class="now">￥{{ item.actualPrice }}</span>
                            <span class="old">￥{{ item.comparativePrice }}</span>
                        </p>
                        <div class="sell">
                            <span>热卖中</span>
                            <span>剩余{{ item.stock }}件</span>
                        </div>
                    </div>
                </div>

            </div>

        </vue-better-scroll>
    </div>
</template>

<script>
    //接口 http://123.207.32.32:8000/home/multidata
    //get 请求
    // type: sell / pop  String  N非必传
    //page: 1            Int     N非必传
    import axios from '../../api';
    import VueBetterScroll from '../../components/Scroll'

    export default {
        name: "Home",
        data: () => ({
            // 这个配置可以开启滚动条，默认为 false。当设置为 true 或者是一个 Object 的时候，都会开启滚动条，默认是会 fade 的
            scrollbarObj: {
                fade: true
            },
            // 这个配置用于做下拉刷新功能，默认为 false。当设置为 true 或者是一个 Object 的时候，可以开启下拉刷新，可以配置顶部下拉的距离（threshold） 来决定刷新时机以及回弹停留的距离（stop）
            pullDownRefreshObj: {
                threshold: 90,
                stop: 40,
                txt: '刷新了哈'
            },
            // 这个配置用于做上拉加载功能，默认为 false。当设置为 true 或者是一个 Object 的时候，可以开启上拉加载，可以配置离底部距离阈值（threshold）来决定开始加载的时机
            pullUpLoadObj: {
                threshold: 0,
                txt: {
                    more: '加载更多...',
                    noMore: '没有更多数据啦'
                }
            },
            startY: 0, // 纵轴方向初始化位置
            scrollToX: 0,
            scrollToY: 0,
            scrollToTime: 700,
            goodsList: [],
            pageIndex: 1,
        }),
        components: {VueBetterScroll},
        created() {

        },
        mounted() {
            this.onPullingDown();
        },
        methods: {
            async getHomeList(pageIndex) {
                //https://nfxts.520shq.com/localQuickPurchase/ogMongoAction/queryByGId?genreId=1&pageSize=5&pageIndex=11
                //const url = 'https://nfxts.520shq.com/localQuickPurchase/selectionGoods/v2/selectionGoodsCom';
                const url = 'https://nfxts.520shq.com/localQuickPurchase/ogMongoAction/queryByGId?genreId=1';
                const res = await axios.get(url, {
                        params:{
                            pageIndex,
                            pageSize: 10
                        }
                });

                // console.log(res, '222222222222222');
                if (res.code === 200) {
                    this.pageIndex++;
                    this.goodsList.push(...res.data.list);
                    this.$refs.scroll.forceUpdate(true)
                }
            },

            // 滚动到页面顶部
            scrollTo() {
                this.$refs.scroll.scrollTo(this.scrollToX, this.scrollToY, this.scrollToTime)
            },
            onPullingDown() {
                // 模拟下拉刷新
                this.pageIndex = 1;
                console.log('下拉刷新',this.pageIndex);
                this.getHomeList(this.pageIndex);
                // count = 0
                // this.getData().then(res => {
                //     this.items = res
                //     this.$refs.scroll.forceUpdate(true)
                // })
            },
            onPullingUp() {
                // 模拟上拉 加载更多数据
                console.log('上拉加载', this.pageIndex);
                this.getHomeList(this.pageIndex);
                // this.getData().then(res => {
                //     this.items = this.items.concat(res)
                //     if (count < 40) {
                //         this.$refs.scroll.forceUpdate(true)
                //     } else {
                //         this.$refs.scroll.forceUpdate(false)
                //     }
                // })
            }
        },
    }
</script>

<style scoped lang="scss">
    .position-box {
        position: fixed;
        top: 0px;
        left: 0;
        right: 0;
        bottom: 50px;
    }

    .goods-list {
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
        padding: 0 7px;

        .goods-item {
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            // min-height: 293px;
            width: 49%;
            margin-top: 6px;
            border: 1px solid #eee;
            box-shadow: 0 0 5px #cecece;
            border-radius: 5px;

            img {
                width: 100%;
            }

            .title {
                font-size: 14px;
                color: #666;
                overflow: hidden;
                text-overflow: ellipsis;
                display: -webkit-box;
                -webkit-line-clamp: 2;
                -webkit-box-orient: vertical;
            }

            .info {
                background-color: #eee;

                .price {
                    margin: 0;

                    .now {
                        color: #f40;
                        font-size: 16px;
                    }

                    .old {
                        text-decoration: line-through;
                        font-size: 12px;
                        margin-left: 15px;
                    }
                }

                .sell {
                    display: flex;
                    justify-content: space-between;
                    font-size: 12px;
                }
            }
        }
    }

</style>