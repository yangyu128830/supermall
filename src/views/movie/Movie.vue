<template>
  <div class="movie-container">
    <!-- 顶部分类标签 -->
    <div class="category-tabs">
      <div 
        v-for="(category, index) in categories" 
        :key="index"
        class="category-tab"
        :class="{ active: currentCategory === index }"
        @click="switchCategory(index)"
      >
        {{ category.name }}
      </div>
    </div>

    <!-- 电影分类内容 -->
    <div v-if="currentCategory === 0" class="category-content">
      <!-- 热门电影 -->
      <section class="section">
        <h2 class="section-title">热门电影</h2>
        <div class="movie-grid">
          <div v-for="movie in hotMovies" :key="movie.id" class="movie-item">
            <img :src="movie.image" alt="" class="movie-poster">
            <div class="movie-info">
              <h3 class="movie-title">{{ movie.title }}</h3>
              <p class="movie-rating">评分: {{ movie.rating }}</p>
              <p class="movie-price">¥{{ movie.price }}</p>
            </div>
          </div>
        </div>
      </section>

      <!-- 待映电影 -->
      <section class="section">
        <h2 class="section-title">待映电影</h2>
        <div class="movie-grid">
          <div v-for="movie in upcomingMovies" :key="movie.id" class="movie-item">
            <img :src="movie.image" alt="" class="movie-poster">
            <div class="movie-info">
              <h3 class="movie-title">{{ movie.title }}</h3>
              <p class="movie-release">上映时间: {{ movie.releaseDate }}</p>
              <p class="movie-price">¥{{ movie.price }}</p>
            </div>
          </div>
        </div>
      </section>

      <!-- 热门推荐 -->
      <section class="section">
        <h2 class="section-title">热门推荐</h2>
        <div class="movie-grid">
          <div v-for="movie in recommendedMovies" :key="movie.id" class="movie-item">
            <img :src="movie.image" alt="" class="movie-poster">
            <div class="movie-info">
              <h3 class="movie-title">{{ movie.title }}</h3>
              <p class="movie-rating">评分: {{ movie.rating }}</p>
              <p class="movie-price">¥{{ movie.price }}</p>
            </div>
          </div>
        </div>
      </section>
    </div>

    <!-- 演唱会分类内容 -->
    <div v-if="currentCategory === 1" class="category-content">
      <section class="section">
        <h2 class="section-title">热门演唱会</h2>
        <div class="concert-grid">
          <div v-for="concert in concerts" :key="concert.id" class="concert-item">
            <img :src="concert.image" alt="" class="concert-poster">
            <div class="concert-info">
              <h3 class="concert-title">{{ concert.title }}</h3>
              <p class="concert-artist">{{ concert.artist }}</p>
              <p class="concert-time">{{ concert.time }}</p>
              <p class="concert-price">¥{{ concert.price }}</p>
            </div>
          </div>
        </div>
      </section>
    </div>

    <!-- 演出分类内容 -->
    <div v-if="currentCategory === 2" class="category-content">
      <section class="section">
        <h2 class="section-title">热门演出</h2>
        <div class="show-grid">
          <div v-for="show in shows" :key="show.id" class="show-item">
            <img :src="show.image" alt="" class="show-poster">
            <div class="show-info">
              <h3 class="show-title">{{ show.title }}</h3>
              <p class="show-type">{{ show.type }}</p>
              <p class="show-time">{{ show.time }}</p>
              <p class="show-price">¥{{ show.price }}</p>
            </div>
          </div>
        </div>
      </section>
    </div>

    <!-- 脱口秀分类内容 -->
    <div v-if="currentCategory === 3" class="category-content">
      <section class="section">
        <h2 class="section-title">热门脱口秀</h2>
        <div class="talkshow-grid">
          <div v-for="talkshow in talkshows" :key="talkshow.id" class="talkshow-item">
            <img :src="talkshow.image" alt="" class="talkshow-poster">
            <div class="talkshow-info">
              <h3 class="talkshow-title">{{ talkshow.title }}</h3>
              <p class="talkshow-performer">{{ talkshow.performer }}</p>
              <p class="talkshow-time">{{ talkshow.time }}</p>
              <p class="talkshow-price">¥{{ talkshow.price }}</p>
            </div>
          </div>
        </div>
      </section>
    </div>

    <!-- 新电影推荐广告 -->
    <div class="ad-section">
      <h2 class="section-title">新片推荐</h2>
      <div class="ad-banner">
        <img src="https://via.placeholder.com/750x200/3CABFF/FFFFFF?text=新片推荐" alt="新片推荐" class="ad-image">
      </div>
    </div>

    <!-- 优惠团购 -->
    <div class="group-buy-section">
      <h2 class="section-title">优惠团购</h2>
      <div class="group-buy-grid">
        <div v-for="deal in groupBuyDeals" :key="deal.id" class="group-buy-item" :data-discount="deal.discount">
          <img :src="deal.image" alt="" class="group-buy-image">
          <div class="group-buy-info">
            <h3 class="group-buy-title">{{ deal.title }}</h3>
            <p class="group-buy-discount">{{ deal.discount }}</p>
            <p class="group-buy-price">¥{{ deal.price }}</p>
            <p class="group-buy-origin-price">原价: ¥{{ deal.originPrice }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Movie',
  data() {
    return {
      currentCategory: 0,
      categories: [
        { name: '电影' },
        { name: '演唱会' },
        { name: '演出' },
        { name: '脱口秀' }
      ],
      // 电影数据
      hotMovies: [
        { id: 1, title: '复仇者联盟4', rating: 8.5, price: 39.9, image: 'https://via.placeholder.com/200x300/FF6B6B/FFFFFF?text=复仇者联盟4' },
        { id: 2, title: '流浪地球', rating: 8.2, price: 29.9, image: 'https://via.placeholder.com/200x300/4ECDC4/FFFFFF?text=流浪地球' },
        { id: 3, title: '哪吒之魔童降世', rating: 8.7, price: 34.9, image: 'https://via.placeholder.com/200x300/95E1D3/FFFFFF?text=哪吒之魔童降世' },
        { id: 4, title: '我不是药神', rating: 9.0, price: 24.9, image: 'https://via.placeholder.com/200x300/F38181/FFFFFF?text=我不是药神' }
      ],
      upcomingMovies: [
        { id: 5, title: '蜘蛛侠：纵横宇宙', releaseDate: '2023-06-02', price: 39.9, image: 'https://via.placeholder.com/200x300/AA96DA/FFFFFF?text=蜘蛛侠：纵横宇宙' },
        { id: 6, title: '闪电侠', releaseDate: '2023-06-16', price: 36.9, image: 'https://via.placeholder.com/200x300/FCBAD3/FFFFFF?text=闪电侠' },
        { id: 7, title: '变形金刚：超能勇士崛起', releaseDate: '2023-06-09', price: 38.9, image: 'https://via.placeholder.com/200x300/A8D8EA/FFFFFF?text=变形金刚：超能勇士崛起' },
        { id: 8, title: '疯狂元素城', releaseDate: '2023-06-16', price: 32.9, image: 'https://via.placeholder.com/200x300/FFD93D/FFFFFF?text=疯狂元素城' }
      ],
      recommendedMovies: [
        { id: 9, title: '银河护卫队3', rating: 8.5, price: 34.9, image: 'https://via.placeholder.com/200x300/6BCB77/FFFFFF?text=银河护卫队3' },
        { id: 10, title: '速度与激情10', rating: 7.2, price: 36.9, image: 'https://via.placeholder.com/200x300/4D96FF/FFFFFF?text=速度与激情10' },
        { id: 11, title: '灌篮高手', rating: 8.9, price: 32.9, image: 'https://via.placeholder.com/200x300/FF6B9D/FFFFFF?text=灌篮高手' },
        { id: 12, title: '这么多年', rating: 7.8, price: 29.9, image: 'https://via.placeholder.com/200x300/845EF7/FFFFFF?text=这么多年' }
      ],
      // 演唱会数据
      concerts: [
        { id: 1, title: '周杰伦演唱会', artist: '周杰伦', time: '2023-07-15', price: 580, image: 'https://via.placeholder.com/200x300/FF6B6B/FFFFFF?text=周杰伦演唱会' },
        { id: 2, title: '林俊杰演唱会', artist: '林俊杰', time: '2023-08-20', price: 480, image: 'https://via.placeholder.com/200x300/4ECDC4/FFFFFF?text=林俊杰演唱会' },
        { id: 3, title: '陈奕迅演唱会', artist: '陈奕迅', time: '2023-09-10', price: 520, image: 'https://via.placeholder.com/200x300/95E1D3/FFFFFF?text=陈奕迅演唱会' },
        { id: 4, title: '五月天演唱会', artist: '五月天', time: '2023-07-29', price: 450, image: 'https://via.placeholder.com/200x300/F38181/FFFFFF?text=五月天演唱会' }
      ],
      // 演出数据
      shows: [
        { id: 1, title: '开心麻花爆笑舞台剧', type: '舞台剧', time: '2023-06-25', price: 180, image: 'https://via.placeholder.com/200x300/AA96DA/FFFFFF?text=开心麻花爆笑舞台剧' },
        { id: 2, title: '国家宝藏展览', type: '展览', time: '2023-07-01', price: 120, image: 'https://via.placeholder.com/200x300/FCBAD3/FFFFFF?text=国家宝藏展览' },
        { id: 3, title: '芭蕾舞剧《天鹅湖》', type: '芭蕾舞', time: '2023-07-10', price: 280, image: 'https://via.placeholder.com/200x300/A8D8EA/FFFFFF?text=芭蕾舞剧《天鹅湖》' },
        { id: 4, title: '交响音乐会', type: '音乐会', time: '2023-07-18', price: 220, image: 'https://via.placeholder.com/200x300/FFD93D/FFFFFF?text=交响音乐会' }
      ],
      // 脱口秀数据
      talkshows: [
        { id: 1, title: '李诞脱口秀专场', performer: '李诞', time: '2023-06-30', price: 280, image: 'https://via.placeholder.com/200x300/6BCB77/FFFFFF?text=李诞脱口秀专场' },
        { id: 2, title: '池子脱口秀', performer: '池子', time: '2023-07-08', price: 220, image: 'https://via.placeholder.com/200x300/4D96FF/FFFFFF?text=池子脱口秀' },
        { id: 3, title: '呼兰脱口秀专场', performer: '呼兰', time: '2023-07-15', price: 260, image: 'https://via.placeholder.com/200x300/FF6B9D/FFFFFF?text=呼兰脱口秀专场' },
        { id: 4, title: '周奇墨脱口秀', performer: '周奇墨', time: '2023-07-22', price: 240, image: 'https://via.placeholder.com/200x300/845EF7/FFFFFF?text=周奇墨脱口秀' }
      ],
      // 团购数据
      groupBuyDeals: [
        { id: 1, title: '双人电影票套餐', discount: '5折', price: 59.9, originPrice: 119.8, image: 'https://via.placeholder.com/200x150/FF6B6B/FFFFFF?text=双人电影票套餐' },
        { id: 2, title: '演唱会门票+周边', discount: '7折', price: 699, originPrice: 998, image: 'https://via.placeholder.com/200x150/4ECDC4/FFFFFF?text=演唱会门票+周边' },
        { id: 3, title: '脱口秀双人套票', discount: '6折', price: 359.9, originPrice: 599.8, image: 'https://via.placeholder.com/200x150/95E1D3/FFFFFF?text=脱口秀双人套票' },
        { id: 4, title: '演出季通票', discount: '4折', price: 899, originPrice: 2247, image: 'https://via.placeholder.com/200x150/F38181/FFFFFF?text=演出季通票' }
      ]
    }
  },
  methods: {
    switchCategory(index) {
      this.currentCategory = index
    }
  }
}
</script>

<style scoped>
.movie-container {
  background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
  min-height: 100vh;
  padding-bottom: 20px;
}

/* 顶部分类标签 */
.category-tabs {
  display: flex;
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  border-bottom: 1px solid rgba(238, 238, 238, 0.8);
  overflow-x: auto;
  padding: 0 20px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
  position: sticky;
  top: 0;
  z-index: 100;
}

.category-tabs::-webkit-scrollbar {
  display: none;
}

.category-tab {
  padding: 18px 25px;
  font-size: 16px;
  font-weight: 500;
  color: #666;
  white-space: nowrap;
  cursor: pointer;
  position: relative;
  transition: all 0.3s ease;
  border-radius: 25px;
  margin: 5px 5px;
}

.category-tab:hover {
  background-color: rgba(60, 171, 255, 0.1);
  color: #3CABFF;
  transform: translateY(-2px);
}

.category-tab.active {
  color: #fff;
  background: linear-gradient(135deg, #3CABFF 0%, #1E90FF 100%);
  box-shadow: 0 4px 15px rgba(60, 171, 255, 0.3);
  transform: translateY(-2px);
}

.category-tab.active::after {
  display: none;
}

/* 分类内容 */
.category-content {
  padding: 20px;
}

.section {
  margin-bottom: 30px;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 16px;
  padding: 20px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.08);
  backdrop-filter: blur(10px);
}

.section-title {
  font-size: 20px;
  font-weight: bold;
  margin-bottom: 20px;
  color: #333;
  position: relative;
  padding-left: 15px;
}

.section-title::before {
  content: '';
  position: absolute;
  left: 0;
  top: 50%;
  transform: translateY(-50%);
  width: 4px;
  height: 20px;
  background: linear-gradient(135deg, #3CABFF 0%, #1E90FF 100%);
  border-radius: 2px;
}

/* 电影网格布局 */
.movie-grid,
.concert-grid,
.show-grid,
.talkshow-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
  gap: 20px;
}

.movie-item,
.concert-item,
.show-item,
.talkshow-item {
  background: rgba(255, 255, 255, 0.9);
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.08);
  transition: all 0.3s ease;
  position: relative;
  backdrop-filter: blur(10px);
}

.movie-item:hover,
.concert-item:hover,
.show-item:hover,
.talkshow-item:hover {
  transform: translateY(-8px) scale(1.02);
  box-shadow: 0 12px 30px rgba(0, 0, 0, 0.15);
}

.movie-poster,
.concert-poster,
.show-poster,
.talkshow-poster {
  width: 100%;
  height: 220px;
  object-fit: cover;
  transition: all 0.3s ease;
}

.movie-item:hover .movie-poster,
.concert-item:hover .concert-poster,
.show-item:hover .show-poster,
.talkshow-item:hover .talkshow-poster {
  transform: scale(1.05);
}

.movie-info,
.concert-info,
.show-info,
.talkshow-info {
  padding: 15px;
}

.movie-title,
.concert-title,
.show-title,
.talkshow-title {
  font-size: 15px;
  font-weight: bold;
  margin-bottom: 8px;
  color: #333;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  line-height: 1.4;
}

.movie-rating,
.movie-release,
.movie-price,
.concert-artist,
.concert-time,
.concert-price,
.show-type,
.show-time,
.show-price,
.talkshow-performer,
.talkshow-time,
.talkshow-price {
  font-size: 12px;
  color: #666;
  margin-bottom: 4px;
  display: flex;
  align-items: center;
}

.movie-rating::before {
  content: '⭐';
  margin-right: 4px;
}

.movie-price,
.concert-price,
.show-price,
.talkshow-price {
  color: #FF6B6B;
  font-weight: bold;
  font-size: 14px;
  margin-top: 8px;
}

/* 广告区域 */
.ad-section {
  padding: 20px;
  margin-bottom: 30px;
}

.ad-banner {
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
  position: relative;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}

.ad-banner::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.1);
  z-index: 1;
}

.ad-image {
  width: 100%;
  height: auto;
  display: block;
  transition: all 0.3s ease;
}

.ad-banner:hover .ad-image {
  transform: scale(1.05);
}

/* 团购区域 */
.group-buy-section {
  padding: 20px;
  margin-bottom: 30px;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 16px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.08);
  backdrop-filter: blur(10px);
}

.group-buy-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(170px, 1fr));
  gap: 20px;
}

.group-buy-item {
  background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.08);
  transition: all 0.3s ease;
  position: relative;
}

.group-buy-item::before {
  content: attr(data-discount);
  position: absolute;
  top: 10px;
  right: 10px;
  background: linear-gradient(135deg, #FF6B6B 0%, #FF8E8E 100%);
  color: #fff;
  padding: 4px 12px;
  border-radius: 20px;
  font-size: 12px;
  font-weight: bold;
  z-index: 2;
}

.group-buy-item:hover {
  transform: translateY(-8px) scale(1.02);
  box-shadow: 0 12px 30px rgba(0, 0, 0, 0.15);
}

.group-buy-image {
  width: 100%;
  height: 130px;
  object-fit: cover;
  transition: all 0.3s ease;
}

.group-buy-item:hover .group-buy-image {
  transform: scale(1.05);
}

.group-buy-info {
  padding: 15px;
}

.group-buy-title {
  font-size: 15px;
  font-weight: bold;
  margin-bottom: 8px;
  color: #333;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  line-height: 1.4;
}

.group-buy-discount {
  display: none;
}

.group-buy-price {
  font-size: 18px;
  color: #FF6B6B;
  font-weight: bold;
  margin-bottom: 4px;
}

.group-buy-origin-price {
  font-size: 12px;
  color: #999;
  text-decoration: line-through;
  display: flex;
  align-items: center;
}

/* 响应式设计 */
@media (max-width: 768px) {
  .category-tabs {
    padding: 0 10px;
  }
  
  .category-tab {
    padding: 15px 20px;
    font-size: 15px;
    margin: 5px 3px;
  }
  
  .category-content {
    padding: 15px;
  }
  
  .section {
    padding: 15px;
    margin-bottom: 20px;
  }
  
  .section-title {
    font-size: 18px;
  }
  
  .movie-grid,
  .concert-grid,
  .show-grid,
  .talkshow-grid {
    grid-template-columns: repeat(auto-fill, minmax(130px, 1fr));
    gap: 15px;
  }
  
  .group-buy-grid {
    grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
    gap: 15px;
  }
  
  .movie-poster,
  .concert-poster,
  .show-poster,
  .talkshow-poster {
    height: 180px;
  }
}

@media (max-width: 480px) {
  .movie-grid,
  .concert-grid,
  .show-grid,
  .talkshow-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 12px;
  }
  
  .group-buy-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 12px;
  }
  
  .movie-poster,
  .concert-poster,
  .show-poster,
  .talkshow-poster {
    height: 160px;
  }
}
</style>