<template>
  <tab-bar>
    <tab-bar-item link="/home">
      <img slot="icon" src="~assets/img/tabbar/home.svg" alt="">
      <img slot="active-icon" src="~assets/img/tabbar/home_active.svg" alt="">
      <div slot="text">首页</div>
    </tab-bar-item>
    <tab-bar-item link="/category">
      <img slot="icon" src="~assets/img/tabbar/category.svg" alt="">
      <img slot="active-icon" src="~assets/img/tabbar/category_active.svg" alt="">
      <div slot="text">分类</div>
    </tab-bar-item>
    <tab-bar-item link="/cart">
      <img slot="icon" src="~assets/img/tabbar/cart.svg" alt="">
      <img slot="active-icon" src="~assets/img/tabbar/cart_active.svg" alt="">
      <div slot="text">购物车</div>
    </tab-bar-item>
    <tab-bar-item link="/sale">
      <svg slot="icon" class="tabbar-icon">
        <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#sale"></use>
      </svg>
      <svg slot="active-icon" class="tabbar-icon">
        <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#sale-active"></use>
      </svg>
      <div slot="text">特价</div>
    </tab-bar-item>
    <tab-bar-item link="/profile">
      <img slot="icon" src="~assets/img/tabbar/profile.svg" alt="">
      <img slot="active-icon" src="~assets/img/tabbar/profile_active.svg" alt="">
      <div slot="text">我的</div>
    </tab-bar-item>
    <tab-bar-item link="/member">
      <svg slot="icon" class="tabbar-icon">
        <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#member"></use>
      </svg>
      <svg slot="active-icon" class="tabbar-icon">
        <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#member-active"></use>
      </svg>
      <div slot="text">会员中心</div>
    </tab-bar-item>
  </tab-bar>
</template>

  <script>
    import TabBar from 'common/tabbar/TabBar'
    import TabBarItem from 'common/tabbar/TabBarItem'

    export default {
      name: "MainTabBar",
      components: {
        TabBar, TabBarItem
      }
    }
  </script>

<style scoped>
.tabbar-icon {
  width: 24px;
  height: 24px;
  fill: #999;
}

.tabbar-icon.active {
  fill: #3CABFF;
}
</style>
