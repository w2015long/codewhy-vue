<template>
    <div class="tab-bar-item" @click="itemClick">
        <div v-if="!isActive">
            <slot name="item-icon"></slot>
        </div>
        <div v-else :style="activeStyle">
            <slot name="item-icon-active"></slot>
        </div>
        <div :style="activeStyle">
            <slot name="item-text"></slot>
        </div>
    </div>
</template>
<script>
    export default {
        name: "TabBarItem",
        props:{
            path: String,  // 当前item对应的路由，由调用者指定
            activeColor:{  // 当前item的文字在活跃时的颜色，默认红色，可由使用者指定
                type:String,
                default:"red"
            }
        },
        data:()=> ({

        }),

        computed: {
            // 判断当前item是否处于活跃状态
            isActive() {
                return this.$route.path.indexOf(this.path) > -1;
            },
            // 计算属性,如果处于活跃状态则设置style，否则去除style
            activeStyle() {
                return this.isActive ? {color: this.activeColor} : {};
            }
        },
        methods:{
            itemClick() {
                if (this.$route.path !== this.path) {
                    this.$router.push(this.path);
                }
            }
        }

    }
</script>

<style scoped>

    .tab-bar-item {
        flex: 1;
        text-align: center;
        height: 49px;
        font-size: 10px;
    }

    .tab-bar-item img {
        margin-top: 4px;
        margin-bottom: 2px;
        width: 22px;
        height: 22px;
        vertical-align: middle;
    }

</style>