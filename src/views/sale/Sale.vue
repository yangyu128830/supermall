<template>
  <div id="sale">
    <!-- 导航栏 -->
    <nav-bar class="nav-bar">
      <div slot="center">特价</div>
    </nav-bar>
    
    <!-- 搜索框 -->
    <div class="search-container">
      <input type="text" class="search-input" placeholder="搜索特价商品">
    </div>
    
    <!-- 天天低价商品框 -->
    <div class="daily-deals">
      <div class="daily-deals-header">
        <h2>天天低价</h2>
        <button class="view-all-btn">查看全部</button>
      </div>
      <div class="daily-deals-list">
        <div class="deal-item" v-for="(item, index) in dailyDeals" :key="index">
          <img :src="item.image" alt="" class="deal-image">
          <div class="deal-info">
            <h3 class="deal-title">{{ item.title }}</h3>
            <p class="deal-price">¥{{ item.price }}</p>
          </div>
        </div>
      </div>
    </div>
    
    <!-- 分类表 -->
    <div class="category-tabs">
      <div class="tab-item" v-for="(category, index) in categories" :key="index" :class="{ active: currentCategory === index }" @click="switchCategory(index)">
        {{ category }}
      </div>
    </div>
    
    <!-- 商品列表 -->
    <scroll class="content" ref="scroll" :probe-type="3">
      <div class="goods-list">
        <div class="goods-item" v-for="(item, index) in currentGoods" :key="index" @click="goToDetail(item.id)">
          <img :src="item.image" alt="" class="goods-image">
          <div class="goods-info">
            <h3 class="goods-title">{{ item.title }}</h3>
            <p class="goods-price">¥{{ item.price }}</p>
            <p class="goods-origin-price">¥{{ item.originPrice }}</p>
          </div>
        </div>
      </div>
    </scroll>
    
    <!-- 我的爆料板块 -->
    <div class="爆料-section">
      <div class="爆料-header">
        <h2>我的爆料</h2>
        <button class="submit-btn">发布爆料</button>
      </div>
      <div class="爆料-list">
        <div class="爆料-item" v-for="(item, index) in 爆料s" :key="index">
          <img :src="item.image" alt="" class="爆料-image">
          <div class="爆料-info">
            <h3 class="爆料-title">{{ item.title }}</h3>
            <p class="爆料-price">¥{{ item.price }}</p>
            <p class="爆料-user">by {{ item.user }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import NavBar from 'common/navbar/NavBar'
import Scroll from 'common/scroll/Scroll'

export default {
  name: "Sale",
  components: {
    NavBar,
    Scroll
  },
  data() {
    return {
      // 天天低价商品数据
      dailyDeals: [
        { id: 1, title: '商品1', price: 9.9, image: 'https://via.placeholder.com/100' },
        { id: 2, title: '商品2', price: 19.9, image: 'https://via.placeholder.com/100' },
        { id: 3, title: '商品3', price: 29.9, image: 'https://via.placeholder.com/100' },
        { id: 4, title: '商品4', price: 39.9, image: 'https://via.placeholder.com/100' }
      ],
      // 分类数据
      categories: ['精选', '24h最热', '3h最热', '好价活动', '食品', '居家'],
      currentCategory: 0,
      // 商品数据
      goodsData: {
        0: [ // 精选
          { id: 1, title: '精选商品1', price: 9.9, originPrice: 19.9, image: 'https://via.placeholder.com/150' },
          { id: 2, title: '精选商品2', price: 19.9, originPrice: 29.9, image: 'https://via.placeholder.com/150' },
          { id: 3, title: '精选商品3', price: 29.9, originPrice: 39.9, image: 'https://via.placeholder.com/150' }
        ],
        1: [ // 24h最热
          { id: 4, title: '24h最热商品1', price: 19.9, originPrice: 29.9, image: 'https://via.placeholder.com/150' },
          { id: 5, title: '24h最热商品2', price: 29.9, originPrice: 39.9, image: 'https://via.placeholder.com/150' },
          { id: 6, title: '24h最热商品3', price: 39.9, originPrice: 49.9, image: 'https://via.placeholder.com/150' }
        ],
        2: [ // 3h最热
          { id: 7, title: '3h最热商品1', price: 9.9, originPrice: 19.9, image: 'https://via.placeholder.com/150' },
          { id: 8, title: '3h最热商品2', price: 19.9, originPrice: 29.9, image: 'https://via.placeholder.com/150' },
          { id: 9, title: '3h最热商品3', price: 29.9, originPrice: 39.9, image: 'https://via.placeholder.com/150' }
        ],
        3: [ // 好价活动
          { id: 10, title: '好价活动商品1', price: 19.9, originPrice: 29.9, image: 'https://via.placeholder.com/150' },
          { id: 11, title: '好价活动商品2', price: 29.9, originPrice: 39.9, image: 'https://via.placeholder.com/150' },
          { id: 12, title: '好价活动商品3', price: 39.9, originPrice: 49.9, image: 'https://via.placeholder.com/150' }
        ],
        4: [ // 食品
          { id: 13, title: '食品商品1', price: 9.9, originPrice: 19.9, image: 'https://via.placeholder.com/150' },
          { id: 14, title: '食品商品2', price: 19.9, originPrice: 29.9, image: 'https://via.placeholder.com/150' },
          { id: 15, title: '食品商品3', price: 29.9, originPrice: 39.9, image: 'https://via.placeholder.com/150' }
        ],
        5: [ // 居家
          { id: 16, title: '居家商品1', price: 19.9, originPrice: 29.9, image: 'https://via.placeholder.com/150' },
          { id: 17, title: '居家商品2', price: 29.9, originPrice: 39.9, image: 'https://via.placeholder.com/150' },
          { id: 18, title: '居家商品3', price: 39.9, originPrice: 49.9, image: 'https://via.placeholder.com/150' }
        ]
      },
      // 爆料数据
      爆料s: [
        { id: 1, title: '爆料商品1', price: 9.9, user: '用户1', image: 'https://via.placeholder.com/100' },
        { id: 2, title: '爆料商品2', price: 19.9, user: '用户2', image: 'https://via.placeholder.com/100' },
        { id: 3, title: '爆料商品3', price: 29.9, user: '用户3', image: 'https://via.placeholder.com/100' }
      ]
    }
  },
  computed: {
    // 当前分类的商品
    currentGoods() {
      return this.goodsData[this.currentCategory]
    }
  },
  methods: {
    // 切换分类
    switchCategory(index) {
      this.currentCategory = index
    },
    // 跳转到商品详情
    goToDetail(id) {
      this.$router.push(`/detail?id=${id}`)
    }
  }
}
</script>

<style scoped>
#sale {
  height: 100vh;
  display: flex;
  flex-direction: column;
}

.nav-bar {
  background-color: #ff6b00;
  color: white;
}

.search-container {
  padding: 10px;
  background-color: #f5f5f5;
}

.search-input {
  width: 100%;
  height: 36px;
  padding: 0 10px;
  border: none;
  border-radius: 18px;
  background-color: white;
  font-size: 14px;
}

.daily-deals {
  padding: 10px;
  background-color: white;
  margin-bottom: 10px;
}

.daily-deals-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

.daily-deals-header h2 {
  font-size: 18px;
  font-weight: bold;
  color: #333;
}

.view-all-btn {
  padding: 5px 10px;
  border: 1px solid #ff6b00;
  border-radius: 15px;
  background-color: white;
  color: #ff6b00;
  font-size: 14px;
  cursor: pointer;
}

.daily-deals-list {
  display: flex;
  overflow-x: auto;
  gap: 10px;
}

.deal-item {
  flex-shrink: 0;
  width: 100px;
  text-align: center;
}

.deal-image {
  width: 100px;
  height: 100px;
  object-fit: cover;
  border-radius: 8px;
  margin-bottom: 5px;
}

.deal-title {
  font-size: 12px;
  color: #333;
  margin-bottom: 3px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.deal-price {
  font-size: 14px;
  font-weight: bold;
  color: #ff6b00;
}

.category-tabs {
  display: flex;
  overflow-x: auto;
  padding: 10px;
  background-color: white;
  margin-bottom: 10px;
  border-bottom: 1px solid #f0f0f0;
}

.tab-item {
  flex-shrink: 0;
  padding: 5px 15px;
  margin-right: 10px;
  border-radius: 15px;
  background-color: #f5f5f5;
  color: #666;
  font-size: 14px;
  cursor: pointer;
  transition: all 0.3s;
}

.tab-item.active {
  background-color: #ff6b00;
  color: white;
}

.content {
  flex: 1;
  overflow: hidden;
}

.goods-list {
  padding: 10px;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 10px;
}

.goods-item {
  background-color: white;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  cursor: pointer;
}

.goods-image {
  width: 100%;
  height: 150px;
  object-fit: cover;
}

.goods-info {
  padding: 10px;
}

.goods-title {
  font-size: 14px;
  color: #333;
  margin-bottom: 5px;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
}

.goods-price {
  font-size: 16px;
  font-weight: bold;
  color: #ff6b00;
  margin-bottom: 3px;
}

.goods-origin-price {
  font-size: 12px;
  color: #999;
  text-decoration: line-through;
}

.爆料-section {
  padding: 10px;
  background-color: white;
  margin-bottom: 10px;
}

.爆料-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

.爆料-header h2 {
  font-size: 18px;
  font-weight: bold;
  color: #333;
}

.submit-btn {
  padding: 5px 10px;
  border: none;
  border-radius: 15px;
  background-color: #ff6b00;
  color: white;
  font-size: 14px;
  cursor: pointer;
}

.爆料-list {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.爆料-item {
  display: flex;
  align-items: center;
  padding: 10px;
  background-color: #f9f9f9;
  border-radius: 8px;
}

.爆料-image {
  width: 80px;
  height: 80px;
  object-fit: cover;
  border-radius: 8px;
  margin-right: 10px;
}

.爆料-info {
  flex: 1;
}

.爆料-title {
  font-size: 14px;
  color: #333;
  margin-bottom: 5px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.爆料-price {
  font-size: 16px;
  font-weight: bold;
  color: #ff6b00;
  margin-bottom: 3px;
}

.爆料-user {
  font-size: 12px;
  color: #999;
}
</style>