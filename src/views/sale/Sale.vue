<template>
  <div class="sale-container">
    <!-- 搜索框 -->
    <div class="search-bar">
      <input type="text" placeholder="搜索特价商品" class="search-input">
      <button class="search-btn">搜索</button>
    </div>

    <!-- 天天低价商品框 -->
    <div class="daily-deals">
      <div class="daily-deals-header">
        <h2>天天低价</h2>
        <button class="view-all-btn">查看全部</button>
      </div>
      <div class="daily-deals-list">
        <div class="product-item" v-for="product in dailyProducts" :key="product.id">
          <img :src="product.image" :alt="product.name" class="product-image">
          <div class="product-info">
            <div class="product-name">{{ product.name }}</div>
            <div class="product-price">¥{{ product.price }}</div>
            <div class="product-original-price">¥{{ product.originalPrice }}</div>
          </div>
        </div>
      </div>
    </div>

    <!-- 分类表 -->
    <div class="category-tabs">
      <div class="category-tab" v-for="category in categories" :key="category.id" :class="{ active: currentCategory === category.id }" @click="switchCategory(category.id)">
        {{ category.name }}
      </div>
    </div>

    <!-- 商品列表 -->
    <div class="product-list">
      <div class="product-item" v-for="product in filteredProducts" :key="product.id">
        <img :src="product.image" :alt="product.name" class="product-image">
        <div class="product-info">
          <div class="product-name">{{ product.name }}</div>
          <div class="product-price">¥{{ product.price }}</div>
          <div class="product-original-price">¥{{ product.originalPrice }}</div>
        </div>
      </div>
    </div>

    <!-- 我的爆料板块 -->
    <div class="my-tips">
      <div class="my-tips-header">
        <h2>我的爆料</h2>
        <button class="add-tip-btn">+ 发布爆料</button>
      </div>
      <div class="my-tips-list">
        <div class="tip-item" v-for="tip in myTips" :key="tip.id">
          <div class="tip-product">
            <img :src="tip.product.image" :alt="tip.product.name" class="tip-product-image">
            <div class="tip-product-info">
              <div class="tip-product-name">{{ tip.product.name }}</div>
              <div class="tip-product-price">¥{{ tip.product.price }}</div>
            </div>
          </div>
          <div class="tip-status">{{ tip.status }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Sale',
  data() {
    return {
      currentCategory: 1,
      categories: [
        { id: 1, name: '精选' },
        { id: 2, name: '24h最热' },
        { id: 3, name: '3h最热' },
        { id: 4, name: '好价活动' },
        { id: 5, name: '食品' },
        { id: 6, name: '居家' }
      ],
      dailyProducts: [
        { id: 1, name: '示例商品1', price: 9.9, originalPrice: 19.9, image: 'https://via.placeholder.com/100' },
        { id: 2, name: '示例商品2', price: 19.9, originalPrice: 39.9, image: 'https://via.placeholder.com/100' },
        { id: 3, name: '示例商品3', price: 29.9, originalPrice: 59.9, image: 'https://via.placeholder.com/100' },
        { id: 4, name: '示例商品4', price: 39.9, originalPrice: 79.9, image: 'https://via.placeholder.com/100' }
      ],
      allProducts: [
        { id: 1, name: '精选商品1', price: 9.9, originalPrice: 19.9, image: 'https://via.placeholder.com/100', category: 1 },
        { id: 2, name: '精选商品2', price: 19.9, originalPrice: 39.9, image: 'https://via.placeholder.com/100', category: 1 },
        { id: 3, name: '24h最热商品1', price: 29.9, originalPrice: 59.9, image: 'https://via.placeholder.com/100', category: 2 },
        { id: 4, name: '24h最热商品2', price: 39.9, originalPrice: 79.9, image: 'https://via.placeholder.com/100', category: 2 },
        { id: 5, name: '3h最热商品1', price: 49.9, originalPrice: 99.9, image: 'https://via.placeholder.com/100', category: 3 },
        { id: 6, name: '3h最热商品2', price: 59.9, originalPrice: 119.9, image: 'https://via.placeholder.com/100', category: 3 },
        { id: 7, name: '好价活动商品1', price: 69.9, originalPrice: 139.9, image: 'https://via.placeholder.com/100', category: 4 },
        { id: 8, name: '好价活动商品2', price: 79.9, originalPrice: 159.9, image: 'https://via.placeholder.com/100', category: 4 },
        { id: 9, name: '食品商品1', price: 89.9, originalPrice: 179.9, image: 'https://via.placeholder.com/100', category: 5 },
        { id: 10, name: '食品商品2', price: 99.9, originalPrice: 199.9, image: 'https://via.placeholder.com/100', category: 5 },
        { id: 11, name: '居家商品1', price: 109.9, originalPrice: 219.9, image: 'https://via.placeholder.com/100', category: 6 },
        { id: 12, name: '居家商品2', price: 119.9, originalPrice: 239.9, image: 'https://via.placeholder.com/100', category: 6 }
      ],
      myTips: [
        { id: 1, product: { id: 1, name: '示例商品1', price: 9.9, image: 'https://via.placeholder.com/100' }, status: '审核中' },
        { id: 2, product: { id: 2, name: '示例商品2', price: 19.9, image: 'https://via.placeholder.com/100' }, status: '已通过' }
      ]
    }
  },
  computed: {
    filteredProducts() {
      return this.allProducts.filter(product => product.category === this.currentCategory)
    }
  },
  methods: {
    switchCategory(categoryId) {
      this.currentCategory = categoryId
    }
  }
}
</script>

<style scoped>
.sale-container {
  padding-bottom: 50px;
}

.search-bar {
  display: flex;
  padding: 10px;
  background-color: #f5f5f5;
}

.search-input {
  flex: 1;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px 0 0 4px;
  font-size: 14px;
}

.search-btn {
  padding: 8px 16px;
  background-color: #3CABFF;
  color: white;
  border: none;
  border-radius: 0 4px 4px 0;
  cursor: pointer;
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
  margin: 0;
  font-size: 18px;
  color: #333;
}

.view-all-btn {
  padding: 5px 10px;
  background-color: #f5f5f5;
  border: 1px solid #ddd;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
}

.daily-deals-list {
  display: flex;
  overflow-x: auto;
}

.product-item {
  flex-shrink: 0;
  width: 120px;
  margin-right: 10px;
  background-color: white;
  border-radius: 4px;
  overflow: hidden;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.product-image {
  width: 100%;
  height: 120px;
  object-fit: cover;
}

.product-info {
  padding: 8px;
}

.product-name {
  font-size: 14px;
  color: #333;
  margin-bottom: 4px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.product-price {
  font-size: 16px;
  color: #ff4444;
  font-weight: bold;
}

.product-original-price {
  font-size: 12px;
  color: #999;
  text-decoration: line-through;
}

.category-tabs {
  display: flex;
  padding: 10px;
  background-color: white;
  margin-bottom: 10px;
  overflow-x: auto;
}

.category-tab {
  flex-shrink: 0;
  padding: 8px 16px;
  margin-right: 10px;
  background-color: #f5f5f5;
  border-radius: 20px;
  cursor: pointer;
  font-size: 14px;
  color: #666;
}

.category-tab.active {
  background-color: #3CABFF;
  color: white;
}

.product-list {
  display: flex;
  flex-wrap: wrap;
  padding: 10px;
  background-color: white;
  margin-bottom: 10px;
}

.product-list .product-item {
  width: calc(50% - 5px);
  margin-right: 10px;
  margin-bottom: 10px;
}

.product-list .product-item:nth-child(even) {
  margin-right: 0;
}

.my-tips {
  padding: 10px;
  background-color: white;
}

.my-tips-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

.my-tips-header h2 {
  margin: 0;
  font-size: 18px;
  color: #333;
}

.add-tip-btn {
  padding: 5px 10px;
  background-color: #3CABFF;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
}

.tip-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  background-color: #f5f5f5;
  border-radius: 4px;
  margin-bottom: 10px;
}

.tip-product {
  display: flex;
  align-items: center;
}

.tip-product-image {
  width: 60px;
  height: 60px;
  object-fit: cover;
  border-radius: 4px;
  margin-right: 10px;
}

.tip-product-info {
  display: flex;
  flex-direction: column;
}

.tip-product-name {
  font-size: 14px;
  color: #333;
  margin-bottom: 4px;
}

.tip-product-price {
  font-size: 16px;
  color: #ff4444;
  font-weight: bold;
}

.tip-status {
  font-size: 14px;
  color: #666;
}
</style>