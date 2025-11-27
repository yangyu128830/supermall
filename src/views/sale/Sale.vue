<template>
  <div id="sale">
    <!-- 顶部导航栏 -->
    <nav-bar class="nav-bar">
      <div slot="center">特价</div>
    </nav-bar>

    <!-- 搜索框 -->
    <div class="search-container">
      <div class="search-box">
        <svg class="search-icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg">
          <path d="M909.6 854.5L649.9 594.8C690.2 542.7 712 479 712 412c0-80.2-31.3-155.4-87.9-212.1-56.6-56.7-132-87.9-212.1-87.9s-155.5 31.3-212.1 87.9C143.2 256.5 112 331.8 112 412c0 80.1 31.3 155.5 87.9 212.1C256.5 680.8 331.8 712 412 712c67 0 130.6-21.8 182.7-62l259.7 259.6c3.2 3.2 8.4 3.2 11.6 0l43.9-43.9c3.2-3.2 3.2-8.4 0-11.6zM570.4 570.4C528 612.7 471.8 636 412 636s-116-23.3-158.4-65.6C211.3 528 188 471.8 188 412s23.3-116.1 65.6-158.4C296 211.3 352.2 188 412 188s116.1 23.2 158.4 65.6S636 352.2 636 412s-23.3 116.1-65.6 158.4z" />
        </svg>
        <input type="text" placeholder="搜索商品" v-model="searchText" @input="onSearch">
      </div>
    </div>

    <!-- 天天低价商品框 -->
    <div class="daily-deals">
      <div class="section-header">
        <h2>天天低价</h2>
        <span class="view-all-btn" @click="viewAllDeals">查看全部 <span class="arrow-right"></span></span>
      </div>
      <div class="daily-deals-list">
        <div class="deal-item" v-for="item in dailyDeals" :key="item.id" @click="goToDetail(item.id)">
          <img v-lazy="item.image" alt="">
          <div class="deal-info">
            <div class="deal-name">{{ item.name }}</div>
            <div class="price-container">
              <span class="current-price">¥{{ item.price }}</span>
              <span class="original-price">¥{{ item.originalPrice }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- 分类表 -->
    <tab-control :titles="categories" @itemClick="selectCategory" class="category-tabs"></tab-control>

    <!-- 商品列表 -->
    <scroll class="content" ref="scroll" @pullingUp="loadMore" :data="showGoodsList" :pull-up-load="true" :probe-type="3">
      <grid-view>
        <div class="goods-item" v-for="item in showGoodsList" :key="item.id" @click="goToDetail(item.id)">
          <img v-lazy="item.image" alt="">
          <div class="goods-info">
            <p class="goods-name">{{ item.name }}</p>
            <div class="price-container">
              <span class="current-price">¥{{ item.price }}</span>
              <span class="original-price">¥{{ item.originalPrice }}</span>
            </div>
            <span class="goods-sales">已售{{ item.sales }}</span>
          </div>
        </div>
      </grid-view>
    </scroll>

    <!-- 我的爆料板块 -->
    <div class="my-tips">
      <div class="section-header">
        <h2>我的爆料</h2>
        <button class="add-tip-btn" @click="addTip">+ 发布爆料</button>
      </div>
      <div class="my-tips-list" v-if="myTips.length > 0">
        <div class="tip-item" v-for="tip in myTips" :key="tip.id">
          <div class="tip-info">
            <div class="tip-title">{{ tip.title }}</div>
            <div class="tip-price">¥{{ tip.price }}</div>
            <div class="tip-time">{{ tip.time }}</div>
          </div>
          <div class="tip-status">{{ tip.status }}</div>
        </div>
      </div>
      <div class="no-tips" v-else>
        <p>您还没有发布过爆料</p>
        <button class="add-tip-btn" @click="addTip">发布您的第一个爆料</button>
      </div>
    </div>

    <back-top @backTop="backTop" class="back-top" v-show="showBackTop">
      <img src="~assets/img/common/top.png" alt="">
    </back-top>
  </div>
</template>

<script>
import NavBar from 'components/common/navbar/NavBar'
import Scroll from 'components/common/scroll/Scroll'
import BackTop from 'components/content/backTop/BackTop'
import TabControl from 'components/content/tabControl/TabControl'
import GridView from 'components/common/gridView/GridView'
import {BACKTOP_DISTANCE} from "@/common/const"

// 模拟数据
const mockDailyDeals = [
  { id: 1, name: '特价商品1', price: 9.9, originalPrice: 19.9, image: 'https://via.placeholder.com/80' },
  { id: 2, name: '特价商品2', price: 19.9, originalPrice: 39.9, image: 'https://via.placeholder.com/80' },
  { id: 3, name: '特价商品3', price: 29.9, originalPrice: 59.9, image: 'https://via.placeholder.com/80' },
  { id: 4, name: '特价商品4', price: 39.9, originalPrice: 79.9, image: 'https://via.placeholder.com/80' },
  { id: 5, name: '特价商品5', price: 49.9, originalPrice: 99.9, image: 'https://via.placeholder.com/80' }
]

const mockGoodsList = {
  '精选': [
    { id: 101, name: '精选商品1', price: 15.9, originalPrice: 29.9, sales: 1234, image: 'https://via.placeholder.com/120' },
    { id: 102, name: '精选商品2', price: 25.9, originalPrice: 49.9, sales: 5678, image: 'https://via.placeholder.com/120' },
    { id: 103, name: '精选商品3', price: 35.9, originalPrice: 69.9, sales: 9012, image: 'https://via.placeholder.com/120' }
  ],
  '24h最热': [
    { id: 201, name: '24h最热商品1', price: 12.9, originalPrice: 24.9, sales: 9876, image: 'https://via.placeholder.com/120' },
    { id: 202, name: '24h最热商品2', price: 22.9, originalPrice: 44.9, sales: 8765, image: 'https://via.placeholder.com/120' },
    { id: 203, name: '24h最热商品3', price: 32.9, originalPrice: 64.9, sales: 7654, image: 'https://via.placeholder.com/120' }
  ],
  '3h最热': [
    { id: 301, name: '3h最热商品1', price: 10.9, originalPrice: 19.9, sales: 6543, image: 'https://via.placeholder.com/120' },
    { id: 302, name: '3h最热商品2', price: 20.9, originalPrice: 39.9, sales: 5432, image: 'https://via.placeholder.com/120' },
    { id: 303, name: '3h最热商品3', price: 30.9, originalPrice: 59.9, sales: 4321, image: 'https://via.placeholder.com/120' }
  ],
  '好价活动': [
    { id: 401, name: '好价活动商品1', price: 18.9, originalPrice: 36.9, sales: 3210, image: 'https://via.placeholder.com/120' },
    { id: 402, name: '好价活动商品2', price: 28.9, originalPrice: 56.9, sales: 2109, image: 'https://via.placeholder.com/120' },
    { id: 403, name: '好价活动商品3', price: 38.9, originalPrice: 76.9, sales: 1098, image: 'https://via.placeholder.com/120' }
  ],
  '食品': [
    { id: 501, name: '食品商品1', price: 11.9, originalPrice: 21.9, sales: 4321, image: 'https://via.placeholder.com/120' },
    { id: 502, name: '食品商品2', price: 21.9, originalPrice: 41.9, sales: 3210, image: 'https://via.placeholder.com/120' },
    { id: 503, name: '食品商品3', price: 31.9, originalPrice: 61.9, sales: 2109, image: 'https://via.placeholder.com/120' }
  ],
  '居家': [
    { id: 601, name: '居家商品1', price: 16.9, originalPrice: 31.9, sales: 5432, image: 'https://via.placeholder.com/120' },
    { id: 602, name: '居家商品2', price: 26.9, originalPrice: 51.9, sales: 4321, image: 'https://via.placeholder.com/120' },
    { id: 603, name: '居家商品3', price: 36.9, originalPrice: 71.9, sales: 3210, image: 'https://via.placeholder.com/120' }
  ]
}

export default {
  name: "Sale",
  components: {
    NavBar,
    Scroll,
    BackTop,
    TabControl,
    GridView
  },
  data() {
    return {
      searchText: '',
      dailyDeals: mockDailyDeals,
      categories: ['精选', '24h最热', '3h最热', '好价活动', '食品', '居家'],
      currentCategory: 0,
      goodsList: mockGoodsList,
      myTips: [],
      showBackTop: false
    }
  },
  computed: {
    showGoodsList() {
      const categoryName = this.categories[this.currentCategory]
      return this.goodsList[categoryName] || []
    }
  },
  methods: {
    onSearch() {
      // 搜索逻辑，这里只是模拟
      console.log('搜索:', this.searchText)
    },
    viewAllDeals() {
      // 查看全部天天低价商品
      console.log('查看全部天天低价商品')
    },
    selectCategory(index) {
      this.currentCategory = index
    },
    goToDetail(id) {
      // 跳转到商品详情页
      this.$router.push({ path: '/detail', query: { id } })
    },
    loadMore() {
      // 加载更多商品
      console.log('加载更多商品')
      this.$refs.scroll.finishPullUp()
    },
    addTip() {
      // 添加爆料
      const newTip = {
        id: Date.now(),
        title: '新的爆料商品',
        price: '9.9',
        time: new Date().toLocaleString(),
        status: '审核中'
      }
      this.myTips.unshift(newTip)
    },
    backTop() {
      this.$refs.scroll.scrollTo(0, 0, 500)
    }
  },
  mounted() {
    // 监听滚动事件
    this.$refs.scroll.$on('scroll', (pos) => {
      this.showBackTop = -pos.y > BACKTOP_DISTANCE
    })
  }
}
</script>

<style scoped>
#sale {
  height: 100vh;
  display: flex;
  flex-direction: column;
  background-color: #f5f5f5;
}

.nav-bar {
  background-color: var(--color-tint);
  color: white;
}

/* 搜索框样式 */
.search-container {
  padding: 10px;
  background-color: white;
}

.search-box {
  display: flex;
  align-items: center;
  background-color: #f0f0f0;
  border-radius: 20px;
  padding: 8px 15px;
}

.search-icon {
  width: 18px;
  height: 18px;
  margin-right: 8px;
  color: #999;
}

.search-box input {
  flex: 1;
  border: none;
  background: transparent;
  outline: none;
  font-size: var(--font-size);
  color: var(--color-text);
}

/* 天天低价样式 */
.daily-deals {
  margin: 10px;
  background-color: white;
  border-radius: 6px;
  overflow: hidden;
}

.section-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px;
  border-bottom: 1px solid #f0f0f0;
}

.section-header h2 {
  font-size: 16px;
  font-weight: bold;
  color: var(--color-text);
  margin: 0;
}

.view-all-btn {
  color: var(--color-high-text);
  font-size: 14px;
  cursor: pointer;
  display: flex;
  align-items: center;
}

.arrow-right {
  border-top: 1px solid var(--color-high-text);
  border-left: 1px solid var(--color-high-text);
  width: 6px;
  height: 6px;
  background-color: transparent;
  transform: rotate(135deg);
  display: inline-block;
  margin-left: 4px;
  margin-top: 2px;
}

.daily-deals-list {
  display: flex;
  overflow-x: auto;
  padding: 15px;
  gap: 15px;
}

.deal-item {
  flex-shrink: 0;
  width: 100px;
  text-align: center;
  cursor: pointer;
}

.deal-item img {
  width: 80px;
  height: 80px;
  object-fit: cover;
  border-radius: 6px;
  margin-bottom: 5px;
}

.deal-info {
  text-align: left;
}

.deal-name {
  font-size: 12px;
  color: var(--color-text);
  margin-bottom: 3px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.price-container {
  display: flex;
  align-items: baseline;
  gap: 5px;
}

.current-price {
  font-size: 14px;
  color: var(--color-high-text);
  font-weight: bold;
}

.original-price {
  font-size: 12px;
  color: #999;
  text-decoration: line-through;
}

/* 分类表样式 */
.category-tabs {
  background-color: white;
  margin-top: 10px;
}

/* 商品列表样式 */
.content {
  flex: 1;
  overflow: hidden;
}

.goods-item {
  background-color: white;
  border-radius: 6px;
  overflow: hidden;
  cursor: pointer;
  position: relative;
  padding-bottom: 40px;
}

.goods-item img {
  width: 100%;
  height: auto;
}

.goods-info {
  font-size: 12px;
  color: var(--color-text);
  position: absolute;
  bottom: 5px;
  left: 0;
  right: 0;
  overflow: hidden;
  text-align: center;
  padding: 0 5px;
}

.goods-name {
  color: var(--color-text);
  margin-bottom: 3px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  font-size: 12px;
}

.goods-sales {
  font-size: 12px;
  color: #999;
  margin-top: 3px;
}

/* 我的爆料样式 */
.my-tips {
  margin: 10px;
  background-color: white;
  border-radius: 6px;
  overflow: hidden;
  margin-bottom: 20px;
}

.add-tip-btn {
  background-color: var(--color-high-text);
  color: white;
  border: none;
  border-radius: 20px;
  padding: 5px 15px;
  font-size: 14px;
  cursor: pointer;
}

.tip-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px;
  border-bottom: 1px solid #f0f0f0;
}

.tip-item:last-child {
  border-bottom: none;
}

.tip-info {
  flex: 1;
}

.tip-title {
  font-size: 14px;
  color: var(--color-text);
  margin-bottom: 3px;
}

.tip-price {
  font-size: 16px;
  color: var(--color-high-text);
  font-weight: bold;
}

.tip-time {
  font-size: 12px;
  color: #999;
}

.tip-status {
  font-size: 12px;
  color: #999;
  background-color: #f0f0f0;
  padding: 3px 8px;
  border-radius: 10px;
}

.no-tips {
  text-align: center;
  padding: 30px 0;
}

.no-tips p {
  color: #999;
  margin-bottom: 15px;
}

.back-top {
  position: fixed;
  bottom: 80px;
  right: 20px;
  z-index: 99;
}

.back-top img {
  width: 40px;
  height: 40px;
}
</style>
