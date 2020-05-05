<template>
    <div id="app">
        <router-view></router-view>
        <tab-bar>
            <tab-bar-item :activeColor="activeColor" path="/home">
                <img slot="item-icon" src="./assets/img/icon_svg/home.svg" alt="">
                <img slot="item-icon-active" src="./assets/img/icon_svg/home-fill.svg" alt="">
                <div slot="item-text">首页</div>
            </tab-bar-item>
            <tab-bar-item :activeColor="activeColor" path="/category">
                <img slot="item-icon" src="./assets/img/icon_svg/category.svg" alt="">
                <img slot="item-icon-active" src="./assets/img/icon_svg/category-fill.svg" alt="">
                <div slot="item-text">分类</div>
            </tab-bar-item>
            <tab-bar-item :activeColor="activeColor" path="/cart">
                <img slot="item-icon" src="./assets/img/icon_svg/cart.svg" alt="">
                <img slot="item-icon-active" src="./assets/img/icon_svg/cart-fill.svg" alt="">
                <div slot="item-text">购物车</div>
            </tab-bar-item>
            <tab-bar-item :activeColor="activeColor" path="/profile">
                <img slot="item-icon" src="./assets/img/icon_svg/me.svg" alt="">
                <img slot="item-icon-active" src="./assets/img/icon_svg/me-fill.svg" alt="">
                <div slot="item-text">我的</div>
            </tab-bar-item>
        </tab-bar>
    </div>
</template>
<script>
    import TabBar from "./components/Tabbar/TabBar";
    import TabBarItem from "./components/TabBarItem/TabBarItem";
    export default {
        name: "App",
        data:()=>({
            activeColor:'#108ee9 '
        }),
        components:{
            TabBar,
            TabBarItem
        }

    }
</script>
<style lang="scss">
    @import "assets/css/reset.css";
</style>
