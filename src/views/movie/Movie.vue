<template>
  <div id="movie">
    <nav-bar class="nav-bar"><div slot="center">电影演出</div></nav-bar>
    <tab-control @itemClick="tabClick" :titles="['电影', '演唱会', '演出', '脱口秀']" class="category-tabs"></tab-control>
    <scroll class="content" ref="scroll" :probe-type="3">
      <div v-if="currentCategory === '电影'">
        <div class="section">
          <h2 class="section-title">热门电影</h2>
          <div class="movie-list">
            <div class="movie-item" v-for="movie in hotMovies" :key="movie.id">
              <img :src="movie.poster" alt="" class="movie-poster">
              <div class="movie-info">
                <h3 class="movie-title">{{ movie.title }}</h3>
                <p class="movie-desc">{{ movie.desc }}</p>
                <div class="movie-rating">评分: {{ movie.rating }}</div>
              </div>
            </div>
          </div>
        </div>
        <div class="section">
          <h2 class="section-title">待映电影</h2>
          <div class="movie-list">
            <div class="movie-item" v-for="movie in upcomingMovies" :key="movie.id">
              <img :src="movie.poster" alt="" class="movie-poster">
              <div class="movie-info">
                <h3 class="movie-title">{{ movie.title }}</h3>
                <p class="movie-desc">{{ movie.desc }}</p>
                <div class="movie-release">上映时间: {{ movie.releaseDate }}</div>
              </div>
            </div>
          </div>
        </div>
        <div class="section">
          <h2 class="section-title">热门推荐</h2>
          <div class="recommend-list">
            <div class="recommend-item" v-for="item in hotRecommendations" :key="item.id">
              <img :src="item.image" alt="" class="recommend-image">
              <div class="recommend-info">
                <h3 class="recommend-title">{{ item.title }}</h3>
                <p class="recommend-desc">{{ item.desc }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div v-else-if="currentCategory === '演唱会'">
        <div class="section">
          <h2 class="section-title">热门演唱会</h2>
          <div class="concert-list">
            <div class="concert-item" v-for="concert in hotConcerts" :key="concert.id">
              <img :src="concert.poster" alt="" class="concert-poster">
              <div class="concert-info">
                <h3 class="concert-title">{{ concert.title }}</h3>
                <p class="concert-artist">{{ concert.artist }}</p>
                <p class="concert-venue">地点: {{ concert.venue }}</p>
                <p class="concert-date">时间: {{ concert.date }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div v-else-if="currentCategory === '演出'">
        <div class="section">
          <h2 class="section-title">热门演出</h2>
          <div class="show-list">
            <div class="show-item" v-for="show in hotShows" :key="show.id">
              <img :src="show.poster" alt="" class="show-poster">
              <div class="show-info">
                <h3 class="show-title">{{ show.title }}</h3>
                <p class="show-type">{{ show.type }}</p>
                <p class="show-venue">地点: {{ show.venue }}</p>
                <p class="show-date">时间: {{ show.date }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div v-else-if="currentCategory === '脱口秀'">
        <div class="section">
          <h2 class="section-title">热门脱口秀</h2>
          <div class="talkshow-list">
            <div class="talkshow-item" v-for="talkshow in hotTalkshows" :key="talkshow.id">
              <img :src="talkshow.poster" alt="" class="talkshow-poster">
              <div class="talkshow-info">
                <h3 class="talkshow-title">{{ talkshow.title }}</h3>
                <p class="talkshow-performer">{{ talkshow.performer }}</p>
                <p class="talkshow-venue">地点: {{ talkshow.venue }}</p>
                <p class="talkshow-date">时间: {{ talkshow.date }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="ad-section">
        <h2 class="section-title">新电影推荐</h2>
        <div class="ad-banner">
          <img src="~assets/img/common/placeholder.png" alt="新电影推荐" class="ad-image">
        </div>
      </div>
      <div class="group-buy-section">
        <h2 class="section-title">优惠团购</h2>
        <div class="group-buy-list">
          <div class="group-buy-item" v-for="item in groupBuyItems" :key="item.id">
            <img :src="item.image" alt="" class="group-buy-image">
            <div class="group-buy-info">
              <h3 class="group-buy-title">{{ item.title }}</h3>
              <p class="group-buy-price">¥{{ item.price }}起</p>
              <p class="group-buy-original-price">原价: ¥{{ item.originalPrice }}</p>
            </div>
          </div>
        </div>
      </div>
    </scroll>
  </div>
</template>

<script>
import NavBar from 'common/navbar/NavBar'
import TabControl from 'content/tabControl/TabControl'
import Scroll from 'common/scroll/Scroll'

export default {
  name: "Movie",
  components: {
    NavBar,
    TabControl,
    Scroll
  },
  data() {
    return {
      currentCategory: '电影',
      hotMovies: [
        {id: 1, title: '复仇者联盟4', desc: '终局之战', rating: 8.5, poster: 'https://via.placeholder.com/150x200'},
        {id: 2, title: '流浪地球', desc: '中国科幻电影', rating: 7.9, poster: 'https://via.placeholder.com/150x200'},
        {id: 3, title: '哪吒之魔童降世', desc: '国产动画电影', rating: 8.3, poster: 'https://via.placeholder.com/150x200'}
      ],
      upcomingMovies: [
        {id: 4, title: '唐人街探案3', desc: '悬疑喜剧', releaseDate: '2021-02-12', poster: 'https://via.placeholder.com/150x200'},
        {id: 5, title: '姜子牙', desc: '国产动画', releaseDate: '2020-10-01', poster: 'https://via.placeholder.com/150x200'}
      ],
      hotRecommendations: [
        {id: 6, title: '电影推荐1', desc: '精彩电影推荐', image: 'https://via.placeholder.com/300x150'},
        {id: 7, title: '电影推荐2', desc: '更多精彩电影', image: 'https://via.placeholder.com/300x150'}
      ],
      hotConcerts: [
        {id: 8, title: '周杰伦演唱会', artist: '周杰伦', venue: '北京鸟巢', date: '2021-05-20', poster: 'https://via.placeholder.com/150x200'},
        {id: 9, title: '林俊杰演唱会', artist: '林俊杰', venue: '上海体育场', date: '2021-06-10', poster: 'https://via.placeholder.com/150x200'}
      ],
      hotShows: [
        {id: 10, title: '开心麻花话剧', type: '话剧', venue: '北京喜剧院', date: '2021-04-15', poster: 'https://via.placeholder.com/150x200'},
        {id: 11, title: '国家大剧院歌剧', type: '歌剧', venue: '国家大剧院', date: '2021-05-01', poster: 'https://via.placeholder.com/150x200'}
      ],
      hotTalkshows: [
        {id: 12, title: '李诞脱口秀', performer: '李诞', venue: '上海笑果工厂', date: '2021-03-20', poster: 'https://via.placeholder.com/150x200'},
        {id: 13, title: '池子脱口秀', performer: '池子', venue: '北京脱口秀俱乐部', date: '2021-04-05', poster: 'https://via.placeholder.com/150x200'}
      ],
      groupBuyItems: [
        {id: 14, title: '电影票团购', price: 29.9, originalPrice: 59.9, image: 'https://via.placeholder.com/150x200'},
        {id: 15, title: '演唱会门票团购', price: 199, originalPrice: 399, image: 'https://via.placeholder.com/150x200'}
      ]
    }
  },
  methods: {
    tabClick(index) {
      const categories = ['电影', '演唱会', '演出', '脱口秀']
      this.currentCategory = categories[index]
    }
  }
}
</script>

<style scoped>
#movie {
  height: 100vh;
  display: flex;
  flex-direction: column;
  background-color: #f5f7fa;
}

.nav-bar {
  background: linear-gradient(135deg, #3CABFF 0%, #667eea 100%);
  color: white;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.category-tabs {
  background-color: white;
  border-bottom: 1px solid #e0e0e0;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
}

.content {
  flex: 1;
  overflow: hidden;
  padding: 10px;
}

.section {
  padding: 20px;
  background-color: white;
  margin-bottom: 20px;
  border-radius: 12px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.06);
  transition: all 0.3s ease;
}

.section:hover {
  transform: translateY(-3px);
  box-shadow: 0 6px 25px rgba(0, 0, 0, 0.12);
}

.section-title {
  font-size: 22px;
  font-weight: 600;
  margin-bottom: 20px;
  color: #2c3e50;
  position: relative;
  padding-left: 15px;
  display: flex;
  align-items: center;
}

.section-title::before {
  content: '';
  position: absolute;
  left: 0;
  top: 50%;
  transform: translateY(-50%);
  width: 5px;
  height: 24px;
  background: linear-gradient(135deg, #3CABFF 0%, #667eea 100%);
  border-radius: 3px;
}

.movie-list,
.concert-list,
.show-list,
.talkshow-list,
.group-buy-list {
  display: flex;
  flex-wrap: wrap;
  gap: 18px;
}

.recommend-list {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.movie-item,
.concert-item,
.show-item,
.talkshow-item,
.group-buy-item {
  width: calc(50% - 9px);
  background-color: white;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.06);
  transition: all 0.3s ease;
}

.recommend-item {
  display: flex;
  background-color: white;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.06);
  transition: all 0.3s ease;
}

.movie-item:hover,
.concert-item:hover,
.show-item:hover,
.talkshow-item:hover,
.recommend-item:hover,
.group-buy-item:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15);
}

.movie-poster,
.concert-poster,
.show-poster,
.talkshow-poster,
.group-buy-image {
  width: 100%;
  height: 220px;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.recommend-image {
  width: 120px;
  height: 80px;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.movie-item:hover .movie-poster,
.concert-item:hover .concert-poster,
.show-item:hover .show-poster,
.talkshow-item:hover .talkshow-poster,
.recommend-item:hover .recommend-image,
.group-buy-item:hover .group-buy-image {
  transform: scale(1.05);
}

.movie-info,
.concert-info,
.show-info,
.talkshow-info,
.group-buy-info {
  padding: 12px;
}

.recommend-info {
  flex: 1;
  padding: 12px;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.movie-title,
.concert-title,
.show-title,
.talkshow-title,
.recommend-title,
.group-buy-title {
  font-size: 16px;
  font-weight: 600;
  margin-bottom: 6px;
  color: #2c3e50;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
}

.movie-desc,
.concert-artist,
.show-type,
.talkshow-performer,
.recommend-desc {
  font-size: 13px;
  color: #7f8c8d;
  margin-bottom: 6px;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 1;
  -webkit-box-orient: vertical;
}

.movie-rating,
.movie-release,
.concert-venue,
.concert-date,
.show-venue,
.show-date,
.talkshow-venue,
.talkshow-date,
.group-buy-price,
.group-buy-original-price {
  font-size: 12px;
  margin-bottom: 4px;
}

.movie-rating {
  color: #f39c12;
  font-weight: 500;
  display: flex;
  align-items: center;
}

.movie-release {
  color: #3498db;
  font-weight: 500;
}

.concert-venue,
.show-venue,
.talkshow-venue {
  color: #95a5a6;
}

.concert-date,
.show-date,
.talkshow-date {
  color: #e74c3c;
  font-weight: 500;
}

.group-buy-price {
  color: #e74c3c;
  font-weight: 600;
  font-size: 16px;
}

.group-buy-original-price {
  color: #bdc3c7;
  text-decoration: line-through;
}

.ad-section {
  padding: 20px;
  background-color: white;
  margin-bottom: 20px;
  border-radius: 12px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.06);
}

.ad-banner {
  width: 100%;
  border-radius: 12px;
  overflow: hidden;
  position: relative;
  height: 180px;
}

.ad-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.ad-banner::before {
  content: '新电影推荐';
  position: absolute;
  top: 15px;
  left: 15px;
  background: rgba(255, 255, 255, 0.95);
  padding: 8px 15px;
  border-radius: 8px;
  font-size: 14px;
  font-weight: 600;
  color: #3CABFF;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.group-buy-section {
  padding: 20px;
  background-color: white;
  margin-bottom: 20px;
  border-radius: 12px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.06);
}
</style>