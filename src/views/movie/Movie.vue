<template>
  <div id="movie">
    <!-- 导航栏 -->
    <nav-bar class="nav-bar">
      <div slot="center">电影演出</div>
    </nav-bar>
    
    <!-- 搜索框 -->
    <div class="search-container">
      <input type="text" class="search-input" placeholder="搜索电影、演出、艺人">
    </div>
    
    <!-- 分类标签 -->
    <div class="category-tabs">
      <div class="tab-item" v-for="(category, index) in categories" :key="index" :class="{ active: currentCategory === index }" @click="switchCategory(index)">
        {{ category }}
      </div>
    </div>
    
    <!-- 内容区域 -->
    <scroll class="content" ref="scroll" :probe-type="3" @scroll="contentScroll">
      <!-- 电影分类内容 -->
      <div v-if="currentCategory === 0">
        <!-- 热门电影 -->
        <div class="section">
          <div class="section-header">
            <h2>热门电影</h2>
            <button class="view-all-btn">查看全部</button>
          </div>
          <div class="movie-list">
            <div class="movie-item" v-for="(movie, index) in hotMovies" :key="index">
              <img :src="movie.image" alt="" class="movie-poster">
              <div class="movie-info">
                <h3 class="movie-title">{{ movie.title }}</h3>
                <p class="movie-rating">{{ movie.rating }}分</p>
                <p class="movie-type">{{ movie.type }}</p>
                <p class="movie-price">¥{{ movie.price }}起</p>
              </div>
            </div>
          </div>
        </div>
        
        <!-- 待映电影 -->
        <div class="section">
          <div class="section-header">
            <h2>待映电影</h2>
            <button class="view-all-btn">查看全部</button>
          </div>
          <div class="movie-list">
            <div class="movie-item" v-for="(movie, index) in upcomingMovies" :key="index">
              <img :src="movie.image" alt="" class="movie-poster">
              <div class="movie-info">
                <h3 class="movie-title">{{ movie.title }}</h3>
                <p class="movie-type">{{ movie.type }}</p>
                <p class="movie-release-date">{{ movie.releaseDate }}</p>
                <button class="preorder-btn">预售</button>
              </div>
            </div>
          </div>
        </div>
        
        <!-- 热门推荐 -->
        <div class="section">
          <div class="section-header">
            <h2>热门推荐</h2>
          </div>
          <div class="recommendation-list">
            <div class="recommendation-item" v-for="(item, index) in hotRecommendations" :key="index">
              <img :src="item.image" alt="" class="recommendation-image">
              <div class="recommendation-info">
                <h3 class="recommendation-title">{{ item.title }}</h3>
                <p class="recommendation-desc">{{ item.description }}</p>
                <p class="recommendation-price">¥{{ item.price }}起</p>
              </div>
            </div>
          </div>
        </div>
      </div>
      
      <!-- 演唱会分类内容 -->
      <div v-if="currentCategory === 1">
        <div class="section">
          <div class="section-header">
            <h2>热门演唱会</h2>
            <button class="view-all-btn">查看全部</button>
          </div>
          <div class="concert-list">
            <div class="concert-item" v-for="(concert, index) in hotConcerts" :key="index">
              <img :src="concert.image" alt="" class="concert-image">
              <div class="concert-info">
                <h3 class="concert-title">{{ concert.title }}</h3>
                <p class="concert-artist">{{ concert.artist }}</p>
                <p class="concert-venue">{{ concert.venue }}</p>
                <p class="concert-date">{{ concert.date }}</p>
                <p class="concert-price">¥{{ concert.price }}起</p>
              </div>
            </div>
          </div>
        </div>
      </div>
      
      <!-- 演出分类内容 -->
      <div v-if="currentCategory === 2">
        <div class="section">
          <div class="section-header">
            <h2>热门演出</h2>
            <button class="view-all-btn">查看全部</button>
          </div>
          <div class="show-list">
            <div class="show-item" v-for="(show, index) in hotShows" :key="index">
              <img :src="show.image" alt="" class="show-image">
              <div class="show-info">
                <h3 class="show-title">{{ show.title }}</h3>
                <p class="show-type">{{ show.type }}</p>
                <p class="show-venue">{{ show.venue }}</p>
                <p class="show-date">{{ show.date }}</p>
                <p class="show-price">¥{{ show.price }}起</p>
              </div>
            </div>
          </div>
        </div>
      </div>
      
      <!-- 脱口秀分类内容 -->
      <div v-if="currentCategory === 3">
        <div class="section">
          <div class="section-header">
            <h2>热门脱口秀</h2>
            <button class="view-all-btn">查看全部</button>
          </div>
          <div class="talkshow-list">
            <div class="talkshow-item" v-for="(talkshow, index) in hotTalkshows" :key="index">
              <img :src="talkshow.image" alt="" class="talkshow-image">
              <div class="talkshow-info">
                <h3 class="talkshow-title">{{ talkshow.title }}</h3>
                <p class="talkshow-performer">{{ talkshow.performer }}</p>
                <p class="talkshow-venue">{{ talkshow.venue }}</p>
                <p class="talkshow-date">{{ talkshow.date }}</p>
                <p class="talkshow-price">¥{{ talkshow.price }}起</p>
              </div>
            </div>
          </div>
        </div>
      </div>
      
      <!-- 新电影推荐广告 -->
      <div class="ad-section">
        <img src="https://via.placeholder.com/375x150/ff6b00/ffffff?text=新电影推荐" alt="新电影推荐" class="ad-image">
      </div>
      
      <!-- 优惠团购 -->
      <div class="section">
        <div class="section-header">
          <h2>优惠团购</h2>
          <button class="view-all-btn">查看全部</button>
        </div>
        <div class="group-buy-list">
          <div class="group-buy-item" v-for="(item, index) in groupBuyItems" :key="index">
            <img :src="item.image" alt="" class="group-buy-image">
            <div class="group-buy-info">
              <h3 class="group-buy-title">{{ item.title }}</h3>
              <p class="group-buy-desc">{{ item.description }}</p>
              <div class="price-container">
                <span class="group-buy-price">¥{{ item.price }}</span>
                <span class="original-price">¥{{ item.originalPrice }}</span>
              </div>
              <div class="progress-container">
                <div class="progress-bar">
                  <div class="progress" :style="{ width: item.progress + '%' }"></div>
                </div>
                <span class="progress-text">{{ item.progress }}%已售</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </scroll>
    
    <!-- 返回顶部按钮 -->
    <back-top @backTop="backTop" class="back-top" v-show="showBackTop">
      <img src="~assets/img/common/top.png" alt="">
    </back-top>
  </div>
</template>

<script>
import NavBar from 'common/navbar/NavBar'
import Scroll from 'common/scroll/Scroll'
import BackTop from 'content/backTop/BackTop'
import {BACKTOP_DISTANCE} from "@/common/const"

export default {
  name: "Movie",
  components: {
    NavBar,
    Scroll,
    BackTop
  },
  data() {
    return {
      // 分类数据
      categories: ['电影', '演唱会', '演出', '脱口秀'],
      currentCategory: 0,
      // 返回顶部按钮显示状态
      showBackTop: false,
      // 热门电影数据
      hotMovies: [
        { id: 1, title: '复仇者联盟4：终局之战', rating: 8.5, type: '动作/科幻', price: 39.9, image: 'https://via.placeholder.com/150x200/ff6b00/ffffff?text=复仇者联盟4' },
        { id: 2, title: '蜘蛛侠：英雄远征', rating: 7.8, type: '动作/冒险', price: 35.9, image: 'https://via.placeholder.com/150x200/ff6b00/ffffff?text=蜘蛛侠' },
        { id: 3, title: '狮子王', rating: 7.5, type: '动画/冒险', price: 32.9, image: 'https://via.placeholder.com/150x200/ff6b00/ffffff?text=狮子王' }
      ],
      // 待映电影数据
      upcomingMovies: [
        { id: 4, title: '冰雪奇缘2', type: '动画/奇幻', releaseDate: '2023-11-22', image: 'https://via.placeholder.com/150x200/ff6b00/ffffff?text=冰雪奇缘2' },
        { id: 5, title: '星球大战9：天行者崛起', type: '动作/科幻', releaseDate: '2023-12-20', image: 'https://via.placeholder.com/150x200/ff6b00/ffffff?text=星球大战9' },
        { id: 6, title: '勇敢者游戏2：再战巅峰', type: '动作/冒险', releaseDate: '2023-12-13', image: 'https://via.placeholder.com/150x200/ff6b00/ffffff?text=勇敢者游戏2' }
      ],
      // 热门推荐数据
      hotRecommendations: [
        { id: 1, title: 'IMAX观影体验', description: '享受震撼的视听效果', price: 59.9, image: 'https://via.placeholder.com/150x100/ff6b00/ffffff?text=IMAX' },
        { id: 2, title: 'VIP厅观影', description: '舒适的观影环境', price: 89.9, image: 'https://via.placeholder.com/150x100/ff6b00/ffffff?text=VIP' },
        { id: 3, title: '双人观影套餐', description: '包含两张电影票和小吃', price: 99.9, image: 'https://via.placeholder.com/150x100/ff6b00/ffffff?text=双人套餐' }
      ],
      // 热门演唱会数据
      hotConcerts: [
        { id: 1, title: '周杰伦演唱会', artist: '周杰伦', venue: '上海体育场', date: '2023-12-31', price: 599, image: 'https://via.placeholder.com/150x100/ff6b00/ffffff?text=周杰伦演唱会' },
        { id: 2, title: '林俊杰演唱会', artist: '林俊杰', venue: '北京鸟巢', date: '2024-01-15', price: 499, image: 'https://via.placeholder.com/150x100/ff6b00/ffffff?text=林俊杰演唱会' },
        { id: 3, title: ' Taylor Swift演唱会', artist: 'Taylor Swift', venue: '广州体育馆', date: '2024-02-20', price: 899, image: 'https://via.placeholder.com/150x100/ff6b00/ffffff?text=Taylor+Swift' }
      ],
      // 热门演出数据
      hotShows: [
        { id: 1, title: '开心麻花话剧', type: '喜剧', venue: '上海话剧艺术中心', date: '2023-11-25', price: 199, image: 'https://via.placeholder.com/150x100/ff6b00/ffffff?text=开心麻花' },
        { id: 2, title: '大河之舞', type: '舞蹈', venue: '北京人民大会堂', date: '2023-12-10', price: 299, image: 'https://via.placeholder.com/150x100/ff6b00/ffffff?text=大河之舞' },
        { id: 3, title: '印象西湖', type: '实景演出', venue: '杭州西湖', date: '2023-11-20', price: 399, image: 'https://via.placeholder.com/150x100/ff6b00/ffffff?text=印象西湖' }
      ],
      // 热门脱口秀数据
      hotTalkshows: [
        { id: 1, title: '李诞脱口秀专场', performer: '李诞', venue: '上海文化广场', date: '2023-12-05', price: 299, image: 'https://via.placeholder.com/150x100/ff6b00/ffffff?text=李诞脱口秀' },
        { id: 2, title: '池子脱口秀', performer: '池子', venue: '北京展览馆剧场', date: '2023-12-18', price: 199, image: 'https://via.placeholder.com/150x100/ff6b00/ffffff?text=池子脱口秀' },
        { id: 3, title: '呼兰脱口秀专场', performer: '呼兰', venue: '广州大剧院', date: '2024-01-08', price: 249, image: 'https://via.placeholder.com/150x100/ff6b00/ffffff?text=呼兰脱口秀' }
      ],
      // 优惠团购数据
      groupBuyItems: [
        { id: 1, title: '双人电影票', description: '两张电影票+两份可乐+一份爆米花', price: 89.9, originalPrice: 159.9, progress: 75, image: 'https://via.placeholder.com/150x100/ff6b00/ffffff?text=双人电影票' },
        { id: 2, title: 'IMAX电影票', description: 'IMAX电影票一张', price: 49.9, originalPrice: 99.9, progress: 60, image: 'https://via.placeholder.com/150x100/ff6b00/ffffff?text=IMAX电影票' },
        { id: 3, title: '演唱会门票', description: '周杰伦演唱会门票一张', price: 499, originalPrice: 599, progress: 85, image: 'https://via.placeholder.com/150x100/ff6b00/ffffff?text=演唱会门票' }
      ]
    }
  },
  methods: {
    // 切换分类
    switchCategory(index) {
      this.currentCategory = index
    },
    
    // 滚动事件处理
    contentScroll(position) {
      // 决定backTop是否显示
      this.showBackTop = position.y < -BACKTOP_DISTANCE
    },
    
    // 返回顶部
    backTop() {
      this.$refs.scroll.scrollTo(0, 0, 300)
    }
  }
}
</script>

<style scoped>
#movie {
  height: 100vh;
  position: relative;
}

/* 导航栏样式 */
.nav-bar {
  background-color: var(--color-tint);
  color: white;
  font-weight: 700;
}

/* 搜索框样式 */
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

/* 分类标签样式 */
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
  background-color: var(--color-tint);
  color: white;
}

/* 内容区域样式 */
.content {
  position: absolute;
  top: 44px;
  bottom: 49px;
  left: 0;
  right: 0;
}

/* 返回顶部按钮样式 */
.back-top {
  position: fixed;
  right: 10px;
  bottom: 60px;
}

/* 章节样式 */
.section {
  background-color: white;
  margin-bottom: 10px;
  padding: 10px;
}

.section-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

.section-header h2 {
  font-size: 18px;
  font-weight: bold;
  color: #333;
}

.view-all-btn {
  padding: 5px 10px;
  border: 1px solid var(--color-tint);
  border-radius: 15px;
  background-color: white;
  color: var(--color-tint);
  font-size: var(--font-size);
  cursor: pointer;
}

/* 电影列表样式 */
.movie-list {
  display: flex;
  overflow-x: auto;
  gap: 10px;
}

.movie-item {
  flex-shrink: 0;
  width: 150px;
  text-align: center;
}

.movie-poster {
  width: 150px;
  height: 200px;
  object-fit: cover;
  border-radius: 8px;
  margin-bottom: 5px;
}

.movie-title {
  font-size: 14px;
  color: #333;
  margin-bottom: 3px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.movie-rating {
  font-size: 12px;
  color: var(--color-tint);
  margin-bottom: 3px;
}

.movie-type {
  font-size: 12px;
  color: #999;
  margin-bottom: 3px;
}

.movie-price {
  font-size: var(--font-size);
  font-weight: bold;
  color: var(--color-tint);
}

.movie-release-date {
  font-size: 12px;
  color: #999;
  margin-bottom: 5px;
}

.preorder-btn {
  padding: 3px 10px;
  border: none;
  border-radius: 10px;
  background-color: var(--color-tint);
  color: white;
  font-size: 12px;
  cursor: pointer;
}

/* 推荐列表样式 */
.recommendation-list {
  display: flex;
  overflow-x: auto;
  gap: 10px;
}

.recommendation-item {
  flex-shrink: 0;
  width: 150px;
  text-align: center;
}

.recommendation-image {
  width: 150px;
  height: 100px;
  object-fit: cover;
  border-radius: 8px;
  margin-bottom: 5px;
}

.recommendation-title {
  font-size: 14px;
  color: #333;
  margin-bottom: 3px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.recommendation-desc {
  font-size: 12px;
  color: #999;
  margin-bottom: 3px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.recommendation-price {
  font-size: var(--font-size);
  font-weight: bold;
  color: var(--color-tint);
}

/* 演唱会列表样式 */
.concert-list {
  display: flex;
  overflow-x: auto;
  gap: 10px;
}

.concert-item {
  flex-shrink: 0;
  width: 150px;
  text-align: center;
}

.concert-image {
  width: 150px;
  height: 100px;
  object-fit: cover;
  border-radius: 8px;
  margin-bottom: 5px;
}

.concert-title {
  font-size: 14px;
  color: #333;
  margin-bottom: 3px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.concert-artist {
  font-size: 12px;
  color: var(--color-tint);
  margin-bottom: 3px;
}

.concert-venue {
  font-size: 12px;
  color: #999;
  margin-bottom: 3px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.concert-date {
  font-size: 12px;
  color: #999;
  margin-bottom: 3px;
}

.concert-price {
  font-size: var(--font-size);
  font-weight: bold;
  color: var(--color-tint);
}

/* 演出列表样式 */
.show-list {
  display: flex;
  overflow-x: auto;
  gap: 10px;
}

.show-item {
  flex-shrink: 0;
  width: 150px;
  text-align: center;
}

.show-image {
  width: 150px;
  height: 100px;
  object-fit: cover;
  border-radius: 8px;
  margin-bottom: 5px;
}

.show-title {
  font-size: 14px;
  color: #333;
  margin-bottom: 3px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.show-type {
  font-size: 12px;
  color: var(--color-tint);
  margin-bottom: 3px;
}

.show-venue {
  font-size: 12px;
  color: #999;
  margin-bottom: 3px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.show-date {
  font-size: 12px;
  color: #999;
  margin-bottom: 3px;
}

.show-price {
  font-size: var(--font-size);
  font-weight: bold;
  color: var(--color-tint);
}

/* 脱口秀列表样式 */
.talkshow-list {
  display: flex;
  overflow-x: auto;
  gap: 10px;
}

.talkshow-item {
  flex-shrink: 0;
  width: 150px;
  text-align: center;
}

.talkshow-image {
  width: 150px;
  height: 100px;
  object-fit: cover;
  border-radius: 8px;
  margin-bottom: 5px;
}

.talkshow-title {
  font-size: 14px;
  color: #333;
  margin-bottom: 3px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.talkshow-performer {
  font-size: 12px;
  color: var(--color-tint);
  margin-bottom: 3px;
}

.talkshow-venue {
  font-size: 12px;
  color: #999;
  margin-bottom: 3px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.talkshow-date {
  font-size: 12px;
  color: #999;
  margin-bottom: 3px;
}

.talkshow-price {
  font-size: var(--font-size);
  font-weight: bold;
  color: var(--color-tint);
}

/* 广告样式 */
.ad-section {
  margin-bottom: 10px;
  padding: 0 10px;
}

.ad-image {
  width: 100%;
  height: 150px;
  object-fit: cover;
  border-radius: 8px;
}

/* 团购列表样式 */
.group-buy-list {
  display: flex;
  overflow-x: auto;
  gap: 10px;
}

.group-buy-item {
  flex-shrink: 0;
  width: 150px;
  text-align: center;
}

.group-buy-image {
  width: 150px;
  height: 100px;
  object-fit: cover;
  border-radius: 8px;
  margin-bottom: 5px;
}

.group-buy-title {
  font-size: 14px;
  color: #333;
  margin-bottom: 3px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.group-buy-desc {
  font-size: 12px;
  color: #999;
  margin-bottom: 5px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.price-container {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 5px;
  margin-bottom: 5px;
}

.group-buy-price {
  font-size: var(--font-size);
  font-weight: bold;
  color: var(--color-tint);
}

.original-price {
  font-size: 12px;
  color: #999;
  text-decoration: line-through;
}

.progress-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.progress-bar {
  width: 100%;
  height: 4px;
  background-color: #f0f0f0;
  border-radius: 2px;
  overflow: hidden;
  margin-bottom: 3px;
}

.progress {
  height: 100%;
  background-color: var(--color-tint);
  border-radius: 2px;
  transition: width 0.3s;
}

.progress-text {
  font-size: 12px;
  color: #999;
}
</style>