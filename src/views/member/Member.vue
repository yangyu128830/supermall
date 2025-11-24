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
          
          <div class="menu-item" @click="goToCollect">
            <div class="menu-icon">
              <img src="~assets/img/common/collect.svg" alt="收藏" class="icon-img">
            </div>
            <div class="menu-content">
              <div class="menu-title">我的收藏</div>
              <div class="menu-subtitle">查看收藏的商品</div>
            </div>
            <div class="arrow-right"></div>
          </div>
          
          <div class="menu-item" @click="goToMessage">
            <div class="menu-icon">
              <img src="~assets/img/profile/message.svg" alt="消息" class="icon-img">
            </div>
            <div class="menu-content">
              <div class="menu-title">消息中心</div>
              <div class="menu-subtitle">查看系统消息</div>
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
          
          <div class="menu-item" @click="goToSetting">
            <div class="menu-icon">
              <img src="~assets/img/profile/cart.svg" alt="设置" class="icon-img">
            </div>
            <div class="menu-content">
              <div class="menu-title">设置</div>
              <div class="menu-subtitle">账号设置</div>
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
                <button class="claim-btn" @click="claimCoupon">立即领取</button>
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
                <button class="claim-btn disabled">已领取</button>
              </div>
            </div>
            
            <div class="benefit-item">
              <div class="benefit-icon">
                <img src="~assets/img/profile/shopping.svg" alt="专属折扣" class="icon-img">
              </div>
              <div class="benefit-content">
                <div class="benefit-title">专属折扣</div>
                <div class="benefit-desc">{{ memberLevel.name }}会员专享9折优惠</div>
              </div>
              <div class="benefit-action">
                <button class="claim-btn">立即查看</button>
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
    }
  },
  methods: {
    claimCoupon() {
      alert(`您已成功领取${this.memberLevel.coupon}元代金券！`);
    },
    goToOrder() {
      console.log('跳转到订单页面');
    },
    goToCoupon() {
      console.log('跳转到优惠券页面');
    },
    goToCollect() {
      console.log('跳转到收藏页面');
    },
    goToMessage() {
      console.log('跳转到消息页面');
    },
    goToAddress() {
      console.log('跳转到地址管理页面');
    },
    goToSetting() {
      console.log('跳转到设置页面');
    }
  }
};
</script>

<style scoped>
#member {
  height: 100vh;
  position: relative;
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
  background-color: var(--color-tint);
  color: #fff;
  padding: 20px;
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
}

.level-tag {
  background-color: rgba(255, 255, 255, 0.3);
  padding: 2px 8px;
  border-radius: 10px;
  margin-right: 5px;
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
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  overflow: hidden;
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

.menu-item:hover {
  background-color: #fafafa;
}

.menu-icon {
  margin-right: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background-color: #f5f5f5;
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
  width: 6px;
  height: 6px;
  border-top: 2px solid #ccc;
  border-right: 2px solid #ccc;
  transform: rotate(45deg);
  margin-left: 10px;
}

.member-benefits,
.member-levels {
  padding: 20px;
  margin-bottom: 10px;
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.member-benefits h3,
.member-levels h3 {
  margin-bottom: 15px;
  font-size: 16px;
  font-weight: bold;
  color: #333;
  border-bottom: 1px solid #f0f0f0;
  padding-bottom: 10px;
}

.benefits-list,
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
}

.benefit-item:last-child {
  border-bottom: none;
}

.benefit-icon {
  margin-right: 15px;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background-color: #f5f5f5;
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

.claim-btn:hover {
  background-color: #ff6600;
}

.claim-btn.disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

.level-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 15px 0;
  border-bottom: 1px solid #f0f0f0;
  position: relative;
  transition: background-color 0.3s ease;
}

.level-item:last-child {
  border-bottom: none;
}

.level-item:hover {
  background-color: #fafafa;
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