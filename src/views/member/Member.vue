<template>
  <div id="member">
    <nav-bar class="nav-bar"><div slot="center">会员中心</div></nav-bar>
    
    <scroll class="content" ref="scroll" :probe-type="3">
      <div>
        <div class="member-header">
          <div class="member-info">
            <div class="member-avatar">
              <img src="~assets/img/profile/avatar.svg" alt="头像" class="avatar-img">
            </div>
            <div class="member-details">
              <div class="member-name">登录/注册</div>
              <div class="member-level">
                <span class="level-tag">{{ memberLevel.name }}</span>
                <span class="level-desc">{{ memberLevel.desc }}</span>
              </div>
              <div class="level-progress">
                <div class="progress-bar">
                  <div class="progress-fill" :style="{ width: progressPercentage + '%' }"></div>
                </div>
                <div class="progress-text">
                  {{ currentPoints }}/{{ nextLevelPoints }} 积分
                </div>
              </div>
            </div>
            <div class="member-points">
              <div class="points-value">{{ currentPoints }}</div>
              <div class="points-label">积分</div>
            </div>
          </div>
        </div>
        
        <div class="member-menu">
          <div class="menu-item" @click="goToOrder">
            <div class="menu-icon">
              <img src="~assets/img/profile/shopping.svg" alt="订单" class="icon-img">
            </div>
            <div class="menu-content">
              <div class="menu-title">我的订单</div>
              <div class="menu-subtitle">查看全部订单</div>
            </div>
            <div class="arrow-right"></div>
          </div>
          
          <div class="menu-item" @click="goToCoupon">
            <div class="menu-icon">
              <img src="~assets/img/profile/pointer.svg" alt="优惠券" class="icon-img">
            </div>
            <div class="menu-content">
              <div class="menu-title">我的优惠券</div>
              <div class="menu-subtitle">{{ couponCount }}张可用</div>
            </div>
            <div class="arrow-right"></div>
          </div>
          
          <div class="menu-item" @click="goToAddress">
            <div class="menu-icon">
              <img src="~assets/img/profile/phone.svg" alt="地址" class="icon-img">
            </div>
            <div class="menu-content">
              <div class="menu-title">收货地址</div>
              <div class="menu-subtitle">管理收货地址</div>
            </div>
            <div class="arrow-right"></div>
          </div>
          
          <div class="menu-item" @click="goToPoints">
            <div class="menu-icon">
              <img src="~assets/img/profile/pointer.svg" alt="积分" class="icon-img">
            </div>
            <div class="menu-content">
              <div class="menu-title">积分商城</div>
              <div class="menu-subtitle">积分兑换好礼</div>
            </div>
            <div class="arrow-right"></div>
          </div>
        </div>
        
        <div class="member-benefits">
          <h3>会员福利</h3>
          <div class="benefits-list">
            <div class="benefit-item">
              <div class="benefit-icon">
                <img src="~assets/img/profile/pointer.svg" alt="代金券" class="icon-img">
              </div>
              <div class="benefit-content">
                <div class="benefit-title">每月代金券</div>
                <div class="benefit-desc">每月可领取{{ memberLevel.coupon }}元代金券</div>
              </div>
              <div class="benefit-action">
                <button class="claim-btn" @click="claimCoupon" :disabled="isCouponClaimed">
                  {{ isCouponClaimed ? '已领取' : '立即领取' }}
                </button>
              </div>
            </div>
            
            <div class="benefit-item">
              <div class="benefit-icon">
                <img src="~assets/img/profile/vip.svg" alt="生日礼" class="icon-img">
              </div>
              <div class="benefit-content">
                <div class="benefit-title">生日福利</div>
                <div class="benefit-desc">生日当月可领取专属礼品</div>
              </div>
              <div class="benefit-action">
                <button class="claim-btn">查看详情</button>
              </div>
            </div>
            
            <div class="benefit-item">
              <div class="benefit-icon">
                <img src="~assets/img/profile/cart.svg" alt="专属折扣" class="icon-img">
              </div>
              <div class="benefit-content">
                <div class="benefit-title">专属折扣</div>
                <div class="benefit-desc">会员专享商品9折优惠</div>
              </div>
              <div class="benefit-action">
                <button class="claim-btn">立即查看</button>
              </div>
            </div>
          </div>
        </div>
        
        <div class="member-activities">
          <h3>会员活动</h3>
          <div class="activities-list">
            <div class="activity-item" v-for="activity in activities" :key="activity.id">
              <div class="activity-img">
                <img :src="activity.img" :alt="activity.title" class="activity-pic">
              </div>
              <div class="activity-content">
                <div class="activity-title">{{ activity.title }}</div>
                <div class="activity-desc">{{ activity.desc }}</div>
                <div class="activity-time">{{ activity.time }}</div>
              </div>
              <div class="activity-action">
                <button class="join-btn">立即参与</button>
              </div>
            </div>
          </div>
        </div>
        
        <div class="member-levels">
          <h3>会员等级说明</h3>
          <div class="levels-list">
            <div class="level-item" :class="{ active: level.name === memberLevel.name }" v-for="level in levels" :key="level.name">
              <div class="level-info">
                <div class="level-name">{{ level.name }}</div>
                <div class="level-requirement">需{{ level.points }}积分</div>
              </div>
              <div class="level-benefits">
                <div class="benefit-item">每月{{ level.coupon }}元代金券</div>
                <div class="benefit-item">专属客服服务</div>
              </div>
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
      couponCount: 3,
      isCouponClaimed: false,
      levels: [
        {
          name: "白银",
          points: 500,
          coupon: 50,
          desc: "白银会员"
        },
        {
          name: "黄金",
          points: 1000,
          coupon: 100,
          desc: "黄金会员"
        },
        {
          name: "黑金",
          points: 10000,
          coupon: 200,
          desc: "黑金会员"
        }
      ],
      activities: [
        {
          id: 1,
          title: "积分翻倍活动",
          desc: "周末购物积分翻倍",
          time: "每周六日",
          img: "~assets/img/common/placeholder.png"
        },
        {
          id: 2,
          title: "新会员专享",
          desc: "新会员首单立减50元",
          time: "永久有效",
          img: "~assets/img/common/placeholder.png"
        },
        {
          id: 3,
          title: "生日特权",
          desc: "生日当月购物享8折优惠",
          time: "生日当月",
          img: "~assets/img/common/placeholder.png"
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
    progressPercentage() {
      const currentIndex = this.levels.findIndex(level => level.name === this.memberLevel.name);
      if (currentIndex === this.levels.length - 1) {
        return 100;
      }
      const currentLevelPoints = this.memberLevel.points;
      const nextLevelPoints = this.levels[currentIndex + 1].points;
      return ((this.currentPoints - currentLevelPoints) / (nextLevelPoints - currentLevelPoints)) * 100;
    }
  },
  methods: {
    claimCoupon() {
      if (this.isCouponClaimed) return;
      this.isCouponClaimed = true;
      this.couponCount++;
      alert(`您已成功领取${this.memberLevel.coupon}元代金券！`);
    },
    goToOrder() {
      console.log('跳转到订单页面');
    },
    goToCoupon() {
      console.log('跳转到优惠券页面');
    },
    goToAddress() {
      console.log('跳转到地址管理页面');
    },
    goToPoints() {
      console.log('跳转到积分商城页面');
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
  font-weight: 700;
  color: #fff;
}

.content {
  position: absolute;
  top: 44px;
  bottom: 49px;
  left: 0;
  right: 0;
}

.member-header {
  background: linear-gradient(135deg, var(--color-tint) 0%, #667eea 100%);
  color: #fff;
  padding: 20px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.member-info {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.member-avatar {
  margin-right: 20px;
}

.avatar-img {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  background-color: rgba(255, 255, 255, 0.3);
  border: 3px solid rgba(255, 255, 255, 0.5);
}

.member-details {
  flex: 1;
}

.member-name {
  font-size: 20px;
  font-weight: bold;
  margin-bottom: 5px;
}

.member-level {
  font-size: 14px;
  margin-bottom: 10px;
}

.level-tag {
  background-color: rgba(255, 255, 255, 0.3);
  padding: 2px 8px;
  border-radius: 10px;
  margin-right: 5px;
}

.level-progress {
  width: 100%;
}

.progress-bar {
  width: 100%;
  height: 6px;
  background-color: rgba(255, 255, 255, 0.3);
  border-radius: 3px;
  overflow: hidden;
  margin-bottom: 5px;
}

.progress-fill {
  height: 100%;
  background-color: #fff;
  border-radius: 3px;
  transition: width 0.3s ease;
}

.progress-text {
  font-size: 12px;
  text-align: right;
}

.member-points {
  text-align: right;
}

.points-value {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 5px;
}

.points-label {
  font-size: 14px;
}

.member-menu {
  background-color: #fff;
  margin-bottom: 10px;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
}

.menu-item {
  display: flex;
  align-items: center;
  padding: 15px 20px;
  border-bottom: 1px solid #f0f0f0;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.menu-item:last-child {
  border-bottom: none;
}

.menu-item:active {
  background-color: #f5f5f5;
}

.menu-icon {
  margin-right: 20px;
}

.icon-img {
  width: 24px;
  height: 24px;
}

.menu-content {
  flex: 1;
  text-align: left;
}

.menu-title {
  font-size: 16px;
  font-weight: bold;
  color: #333;
  margin-bottom: 5px;
}

.menu-subtitle {
  font-size: 14px;
  color: #666;
}

.arrow-right {
  width: 8px;
  height: 8px;
  border-top: 2px solid #ccc;
  border-right: 2px solid #ccc;
  transform: rotate(45deg);
}

.member-benefits,
.member-activities,
.member-levels {
  padding: 20px;
  margin-bottom: 10px;
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
}

.member-benefits h3,
.member-activities h3,
.member-levels h3 {
  margin-bottom: 15px;
  font-size: 16px;
  font-weight: bold;
  color: #333;
  border-bottom: 1px solid #f0f0f0;
  padding-bottom: 10px;
}

.benefits-list,
.activities-list,
.levels-list {
  display: flex;
  flex-direction: column;
}

.benefit-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 15px 0;
  border-bottom: 1px solid #f0f0f0;
  transition: transform 0.3s ease;
}

.benefit-item:last-child {
  border-bottom: none;
}

.benefit-item:active {
  transform: scale(0.98);
}

.benefit-icon {
  margin-right: 15px;
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
}

.claim-btn {
  background-color: var(--color-high-text);
  color: #fff;
  border: none;
  padding: 8px 16px;
  border-radius: 20px;
  font-size: 12px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.claim-btn:active {
  background-color: #409eff;
}

.claim-btn:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

.activity-item {
  display: flex;
  align-items: center;
  padding: 15px 0;
  border-bottom: 1px solid #f0f0f0;
  transition: transform 0.3s ease;
}

.activity-item:last-child {
  border-bottom: none;
}

.activity-item:active {
  transform: scale(0.98);
}

.activity-img {
  margin-right: 15px;
  width: 60px;
  height: 60px;
  border-radius: 8px;
  overflow: hidden;
}

.activity-pic {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.activity-content {
  flex: 1;
}

.activity-title {
  font-size: 14px;
  font-weight: bold;
  color: #333;
  margin-bottom: 5px;
}

.activity-desc {
  font-size: 12px;
  color: #666;
  margin-bottom: 5px;
}

.activity-time {
  font-size: 11px;
  color: #999;
}

.join-btn {
  background-color: var(--color-high-text);
  color: #fff;
  border: none;
  padding: 6px 12px;
  border-radius: 15px;
  font-size: 11px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.join-btn:active {
  background-color: #409eff;
}

.level-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 15px 0;
  border-bottom: 1px solid #f0f0f0;
  position: relative;
  transition: transform 0.3s ease;
}

.level-item:last-child {
  border-bottom: none;
}

.level-item:active {
  transform: scale(0.98);
}

.level-item.active {
  color: var(--color-high-text);
  font-weight: bold;
}

.level-item.active::after {
  content: "当前等级";
  position: absolute;
  top: 10px;
  right: 0;
  background-color: var(--color-high-text);
  color: #fff;
  padding: 2px 8px;
  border-radius: 10px;
  font-size: 12px;
  font-weight: normal;
}

.level-info {
  flex: 1;
}

.level-name {
  font-size: 14px;
  margin-bottom: 5px;
}

.level-requirement {
  font-size: 12px;
  color: #666;
}

.level-benefits {
  text-align: right;
}

.level-benefits .benefit-item {
  font-size: 12px;
  padding: 0;
  border-bottom: none;
}
</style>