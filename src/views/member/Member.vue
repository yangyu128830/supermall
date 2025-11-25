<template>
  <div id="member">
    <nav-bar class="nav-bar"><div slot="center">会员中心</div></nav-bar>
    
    <scroll class="content" ref="scroll" :probe-type="3">
      <div>
        <!-- 会员信息头部 -->
        <div class="member-header">
          <div class="member-avatar">
            <img src="~assets/img/profile/avatar.svg" alt="头像" class="avatar-img">
            <div class="avatar-badge" v-if="unreadNotifications > 0">{{ unreadNotifications }}</div>
          </div>
          <div class="member-info">
            <div class="member-name">登录/注册</div>
            <div class="member-desc">点击登录，享受更多会员权益</div>
          </div>
          <div class="member-arrow">
            <span class="arrow-right"></span>
          </div>
        </div>
        
        <!-- 会员数据统计 -->
        <div class="member-stats">
          <div class="stat-item" v-for="stat in stats" :key="stat.name" @click="handleStatClick(stat)">
            <div class="stat-icon">
              <svg class="icon">
                <use xmlns:xlink="http://www.w3.org/1999/xlink" :xlink:href="`#${stat.icon}`"></use>
              </svg>
            </div>
            <div class="stat-value">{{ stat.value }}</div>
            <div class="stat-name">{{ stat.name }}</div>
          </div>
        </div>
        
        <!-- 会员专属福利 -->
        <div class="member-benefits-section">
          <div class="section-header">
            <h3>会员专属福利</h3>
            <span class="more-btn" @click="showAllBenefits">查看全部 <span class="arrow-right"></span></span>
          </div>
          <div class="benefits-carousel">
            <div class="benefit-card" v-for="benefit in benefits" :key="benefit.id" @click="handleBenefitClick(benefit)">
              <div class="benefit-icon-wrapper" :style="{ backgroundColor: benefit.color }">
                <svg class="icon">
                  <use xmlns:xlink="http://www.w3.org/1999/xlink" :xlink:href="`#${benefit.icon}`"></use>
                </svg>
              </div>
              <div class="benefit-content">
                <div class="benefit-title">{{ benefit.title }}</div>
                <div class="benefit-desc">{{ benefit.desc }}</div>
                <div class="benefit-action" v-if="benefit.available">
                  <button class="claim-btn" :class="{ claimed: benefit.claimed }">
                    {{ benefit.claimed ? '已领取' : '立即领取' }}
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
        
        <!-- 会员功能菜单 -->
        <div class="member-menu">
          <div class="menu-group">
            <div class="menu-group-title">我的服务</div>
            <div class="menu-item" v-for="item in menuItems" :key="item.name" @click="handleMenuClick(item)">
              <div class="menu-icon">
                <svg class="icon">
                  <use xmlns:xlink="http://www.w3.org/1999/xlink" :xlink:href="`#${item.icon}`"></use>
                </svg>
              </div>
              <div class="menu-content">
                <div class="menu-name">{{ item.name }}</div>
                <div class="menu-desc" v-if="item.desc">{{ item.desc }}</div>
              </div>
              <div class="menu-arrow">
                <span class="arrow-right"></span>
              </div>
              <div class="menu-badge" v-if="item.badge">{{ item.badge }}</div>
            </div>
          </div>
          
          <div class="menu-group">
            <div class="menu-group-title">会员权益</div>
            <div class="menu-item" @click="handleMenuClick({ name: '会员等级' })">
              <div class="menu-icon">
                <svg class="icon">
                  <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#level"></use>
                </svg>
              </div>
              <div class="menu-content">
                <div class="menu-name">会员等级</div>
                <div class="menu-desc">{{ memberLevel.name }}会员</div>
              </div>
              <div class="menu-arrow">
                <span class="arrow-right"></span>
              </div>
            </div>
            
            <div class="menu-item" @click="handleMenuClick({ name: '积分中心' })">
              <div class="menu-icon">
                <svg class="icon">
                  <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#point"></use>
                </svg>
              </div>
              <div class="menu-content">
                <div class="menu-name">积分中心</div>
                <div class="menu-desc">{{ currentPoints }}积分</div>
              </div>
              <div class="menu-arrow">
                <span class="arrow-right"></span>
              </div>
            </div>
          </div>
        </div>
        
        <!-- 会员等级信息 -->
        <div class="member-level-card">
          <div class="level-card-header">
            <div class="level-info">
              <div class="current-level">当前等级：{{ memberLevel.name }}</div>
              <div class="level-progress">
                <div class="progress-bar">
                  <div class="progress-fill" :style="{ width: progressPercent + '%' }"></div>
                </div>
                <div class="progress-text">
                  {{ currentPoints }}/{{ nextLevelPoints }}积分
                </div>
              </div>
            </div>
            <div class="level-benefit">
              <div class="benefit-text">每月可领{{ memberLevel.coupon }}元代金券</div>
            </div>
          </div>
          
          <div class="level-benefits-list">
            <div class="level-benefit-item" v-for="(benefit, index) in memberLevel.benefits" :key="index">
              <svg class="benefit-check-icon">
                <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#check"></use>
              </svg>
              <span class="benefit-text">{{ benefit }}</span>
            </div>
          </div>
        </div>
      </div>
    </scroll>
  </div>
</template>

<script>
import NavBar from 'common/navbar/NavBar'
import Scroll from 'common/scroll/Scroll'

export default {
  name: "Member",
  components: {
    NavBar,
    Scroll
  },
  data() {
    return {
      currentPoints: 800,
      unreadNotifications: 3,
      levels: [
        {
          name: "白银",
          points: 500,
          coupon: 50,
          benefits: [
            "每月50元代金券",
            "免费退换货",
            "专属客服"
          ]
        },
        {
          name: "黄金",
          points: 1000,
          coupon: 100,
          benefits: [
            "每月100元代金券",
            "免费退换货",
            "专属客服",
            "生日礼品",
            "优先发货"
          ]
        },
        {
          name: "黑金",
          points: 10000,
          coupon: 200,
          benefits: [
            "每月200元代金券",
            "免费退换货",
            "专属客服",
            "生日礼品",
            "优先发货",
            "专属客户经理",
            "机场贵宾厅"
          ]
        }
      ],
      stats: [
        { name: "我的订单", value: "12", icon: "order" },
        { name: "待支付", value: "2", icon: "payment" },
        { name: "待收货", value: "3", icon: "delivery" },
        { name: "待评价", value: "1", icon: "comment" }
      ],
      menuItems: [
        { name: "优惠券", icon: "coupon", desc: "1张可用" },
        { name: "地址管理", icon: "location" },
        { name: "客服中心", icon: "service" },
        { name: "设置", icon: "setting", badge: "new" }
      ],
      benefits: [
        {
          id: 1,
          title: "新会员专享",
          desc: "满100减50元代金券",
          icon: "coupon",
          color: "#FF6B6B",
          available: true,
          claimed: false
        },
        {
          id: 2,
          title: "生日福利",
          desc: "生日当月可领100元代金券",
          icon: "gift",
          color: "#4ECDC4",
          available: true,
          claimed: false
        },
        {
          id: 3,
          title: "签到有礼",
          desc: "连续签到7天领20元代金券",
          icon: "calendar",
          color: "#FFE66D",
          available: true,
          claimed: true
        },
        {
          id: 4,
          title: "邀请好友",
          desc: "邀请好友注册领50元代金券",
          icon: "share",
          color: "#95E1D3",
          available: true,
          claimed: false
        }
      ]
    };
  },
  computed: {
    memberLevel() {
      let level = this.levels[0];
      for (let i = 0; i < this.levels.length; i++) {
        if (this.currentPoints >= this.levels[i].points) {
          level = this.levels[i];
        } else {
          break;
        }
      }
      return level;
    },
    nextLevelPoints() {
      const currentIndex = this.levels.findIndex(level => level.name === this.memberLevel.name);
      if (currentIndex === this.levels.length - 1) {
        return this.memberLevel.points;
      }
      return this.levels[currentIndex + 1].points;
    },
    progressPercent() {
      const currentIndex = this.levels.findIndex(level => level.name === this.memberLevel.name);
      if (currentIndex === this.levels.length - 1) {
        return 100;
      }
      const currentLevelPoints = this.levels[currentIndex].points;
      const nextLevelPoints = this.levels[currentIndex + 1].points;
      return ((this.currentPoints - currentLevelPoints) / (nextLevelPoints - currentLevelPoints)) * 100;
    }
  },
  methods: {
    handleMenuClick(item) {
      console.log(`点击了${item.name}`);
      // 这里可以添加菜单点击的逻辑
    },
    handleStatClick(stat) {
      console.log(`点击了统计项：${stat.name}`);
      // 这里可以添加统计项点击的逻辑
    },
    handleBenefitClick(benefit) {
      if (benefit.available && !benefit.claimed) {
        benefit.claimed = true;
        this.$toast.show(`已成功领取${benefit.title}`);
      }
    },
    showAllBenefits() {
      console.log("查看全部福利");
      // 这里可以添加查看全部福利的逻辑
    }
  }
};
</script>

<style scoped>
#member {
  height: 100vh;
  position: relative;
  background-color: #f5f5f5;
}

.nav-bar {
  background-color: var(--color-tint);
  color: #fff;
}

.content {
  position: absolute;
  top: 44px;
  bottom: 50px;
  width: 100%;
}

/* 会员信息头部 */
.member-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 20px;
  background-color: #fff;
  margin-bottom: 10px;
  position: relative;
}

.member-avatar {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  overflow: hidden;
  background-color: #eee;
  position: relative;
}

.avatar-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.avatar-badge {
  position: absolute;
  top: -5px;
  right: -5px;
  background-color: #ff4d4f;
  color: #fff;
  font-size: 12px;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  min-width: 20px;
}

.member-info {
  flex: 1;
  margin-left: 15px;
}

.member-name {
  font-size: 18px;
  font-weight: bold;
  color: #333;
  margin-bottom: 5px;
}

.member-desc {
  font-size: 14px;
  color: #999;
}

.member-arrow {
  color: #999;
}

/* 会员数据统计 */
.member-stats {
  display: flex;
  background-color: #fff;
  margin-bottom: 10px;
  padding: 20px 0;
}

.stat-item {
  flex: 1;
  text-align: center;
  cursor: pointer;
  transition: all 0.3s ease;
  padding: 10px;
}

.stat-item:active {
  transform: scale(0.95);
}

.stat-icon {
  width: 30px;
  height: 30px;
  margin: 0 auto 10px;
}

.stat-icon .icon {
  width: 100%;
  height: 100%;
  fill: var(--color-tint);
}

.stat-value {
  font-size: 20px;
  font-weight: bold;
  color: var(--color-high-text);
  margin-bottom: 5px;
}

.stat-name {
  font-size: 14px;
  color: #666;
}

/* 会员专属福利 */
.member-benefits-section {
  background-color: #fff;
  margin-bottom: 10px;
  padding: 20px;
}

.section-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 15px;
}

.section-header h3 {
  font-size: 16px;
  font-weight: bold;
  color: #333;
  margin: 0;
}

.more-btn {
  font-size: 14px;
  color: var(--color-tint);
  display: flex;
  align-items: center;
  cursor: pointer;
}

.more-btn .arrow-right {
  margin-left: 5px;
}

.benefits-carousel {
  display: flex;
  overflow-x: auto;
  gap: 15px;
  padding-bottom: 10px;
}

.benefits-carousel::-webkit-scrollbar {
  display: none;
}

.benefit-card {
  flex-shrink: 0;
  width: 200px;
  background-color: #f8f9fa;
  border-radius: 10px;
  padding: 15px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
  cursor: pointer;
}

.benefit-card:active {
  transform: scale(0.95);
}

.benefit-icon-wrapper {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 10px;
}

.benefit-icon-wrapper .icon {
  width: 24px;
  height: 24px;
  fill: #fff;
}

.benefit-content {
  flex: 1;
}

.benefit-title {
  font-size: 14px;
  font-weight: bold;
  color: #333;
  margin-bottom: 5px;
}

.benefit-desc {
  font-size: 12px;
  color: #666;
  margin-bottom: 10px;
  line-height: 1.4;
}

.benefit-action {
  text-align: center;
}

.claim-btn {
  background-color: var(--color-tint);
  color: #fff;
  border: none;
  padding: 6px 12px;
  border-radius: 15px;
  font-size: 12px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.claim-btn.claimed {
  background-color: #999;
  cursor: not-allowed;
}

/* 会员功能菜单 */
.member-menu {
  background-color: #fff;
  margin-bottom: 10px;
}

.menu-group {
  padding: 10px 0;
  border-bottom: 1px solid #f0f0f0;
}

.menu-group:last-child {
  border-bottom: none;
}

.menu-group-title {
  font-size: 14px;
  color: #999;
  padding: 0 20px 10px;
  margin: 0;
}

.menu-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 15px 20px;
  border-bottom: 1px solid #f0f0f0;
  cursor: pointer;
  transition: all 0.3s ease;
  position: relative;
}

.menu-item:last-child {
  border-bottom: none;
}

.menu-item:active {
  background-color: #f5f5f5;
}

.menu-icon {
  width: 30px;
  height: 30px;
  margin-right: 15px;
}

.menu-icon .icon {
  width: 100%;
  height: 100%;
  fill: #666;
}

.menu-content {
  flex: 1;
}

.menu-name {
  font-size: 16px;
  color: #333;
  margin-bottom: 3px;
}

.menu-desc {
  font-size: 12px;
  color: #999;
}

.menu-arrow {
  color: #999;
}

.menu-badge {
  position: absolute;
  top: 12px;
  right: 40px;
  background-color: #ff4d4f;
  color: #fff;
  font-size: 10px;
  padding: 2px 6px;
  border-radius: 10px;
  min-width: 16px;
  text-align: center;
}

/* 会员等级信息 */
.member-level-card {
  background-color: #fff;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.level-card-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 15px;
}

.current-level {
  font-size: 16px;
  font-weight: bold;
  color: #333;
  margin-bottom: 10px;
}

.level-progress {
  width: 150px;
}

.progress-bar {
  width: 100%;
  height: 6px;
  background-color: #f0f0f0;
  border-radius: 3px;
  overflow: hidden;
  margin-bottom: 5px;
}

.progress-fill {
  height: 100%;
  background-color: var(--color-tint);
  border-radius: 3px;
  transition: width 0.3s ease;
}

.progress-text {
  font-size: 12px;
  color: #999;
  text-align: center;
}

.level-benefit {
  text-align: right;
}

.benefit-text {
  font-size: 14px;
  color: var(--color-high-text);
  font-weight: bold;
}

.level-benefits-list {
  margin-top: 15px;
  padding-top: 15px;
  border-top: 1px solid #f0f0f0;
}

.level-benefit-item {
  display: flex;
  align-items: center;
  margin-bottom: 8px;
  font-size: 14px;
  color: #666;
}

.benefit-check-icon {
  width: 16px;
  height: 16px;
  margin-right: 10px;
  fill: var(--color-tint);
}

.level-benefit-item:last-child {
  margin-bottom: 0;
}
</style>