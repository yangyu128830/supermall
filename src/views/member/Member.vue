<template>
  <div id="member">
    <nav-bar class="nav-bar"><div slot="center">会员中心</div></nav-bar>
    <!-- 会员信息头部 -->
    <div class="member-header">
      <div class="member-avatar">
        <img src="~assets/img/tabbar/profile.svg" alt="头像">
      </div>
      <div class="member-info">
        <h2 class="member-name">超级会员</h2>
        <div class="member-level" :class="memberLevel.class">
          <span class="level-name">{{ memberLevel.name }}</span>
          <span class="level-score">积分：{{ currentScore }}</span>
        </div>
        <div class="level-progress">
          <div class="progress-bar">
            <div class="progress-fill" :style="{ width: progressWidth + '%' }"></div>
          </div>
          <div class="progress-text">
            <span>当前：{{ currentScore }}分</span>
            <span>距离{{ nextLevel.name }}：{{ nextLevel.needScore - currentScore }}分</span>
          </div>
        </div>
      </div>
    </div>

    <!-- 会员福利 -->
    <div class="member-benefits">
      <h3 class="section-title">会员福利</h3>
      <div class="benefits-list">
        <div class="benefit-item" :class="memberLevel.class">
          <div class="benefit-icon">🎁</div>
          <div class="benefit-info">
            <h4>每月代金券</h4>
            <p>{{ memberLevel.coupon }}元</p>
          </div>
          <button class="claim-btn" :disabled="isCouponClaimed" @click="claimCoupon">
            {{ isCouponClaimed ? '已领取' : '领取' }}
          </button>
        </div>
      </div>
    </div>

    <!-- 会员等级说明 -->
    <div class="member-levels">
      <h3 class="section-title">会员等级说明</h3>
      <div class="levels-list">
        <router-link :to="'/member/' + level.name" class="level-item-link" v-for="level in allLevels" :key="level.name">
          <div class="level-item" :class="{ active: level.name === memberLevel.name }">
            <div class="level-icon">{{ level.icon }}</div>
            <div class="level-details">
              <h4>{{ level.name }}</h4>
              <p>所需积分：{{ level.needScore }}分</p>
              <p>福利：每月{{ level.coupon }}元代金券</p>
            </div>
            <span class="arrow-right"></span>
          </div>
        </router-link>
      </div>
    </div>

    <!-- 测试操作区域 -->
    <div class="test-section">
      <h3 class="section-title">测试操作</h3>
      <div class="test-buttons">
        <button class="test-btn" @click="addScore(100)">增加100积分</button>
        <button class="test-btn" @click="addScore(500)">增加500积分</button>
        <button class="test-btn" @click="addScore(1000)">增加1000积分</button>
        <button class="test-btn" @click="resetScore">重置积分</button>
      </div>
    </div>
  </div>
</template>

<script>
import NavBar from 'common/navbar/NavBar'

export default {
  name: 'Member',
  components: {
    NavBar
  },
  data() {
    return {
      currentScore: 800, // 默认积分，可通过测试按钮修改
      isCouponClaimed: false, // 代金券是否已领取
      allLevels: [
        { name: '白银会员', needScore: 500, coupon: 50, class: 'silver', icon: '🥈' },
        { name: '黄金会员', needScore: 1000, coupon: 100, class: 'gold', icon: '🥇' },
        { name: '黑金会员', needScore: 10000, coupon: 200, class: 'black-gold', icon: '💎' }
      ]
    }
  },
  computed: {
    // 当前会员等级
    memberLevel() {
      if (this.currentScore >= 10000) {
        return this.allLevels[2];
      } else if (this.currentScore >= 1000) {
        return this.allLevels[1];
      } else if (this.currentScore >= 500) {
        return this.allLevels[0];
      } else {
        // 非会员
        return { name: '普通会员', needScore: 0, coupon: 0, class: 'normal', icon: '⭐' };
      }
    },
    // 下一个会员等级
    nextLevel() {
      if (this.currentScore < 500) {
        return this.allLevels[0];
      } else if (this.currentScore < 1000) {
        return this.allLevels[1];
      } else if (this.currentScore < 10000) {
        return this.allLevels[2];
      } else {
        // 已经是最高等级
        return this.allLevels[2];
      }
    },
    // 等级进度条宽度
    progressWidth() {
      if (this.currentScore >= 10000) {
        return 100;
      } else if (this.currentScore >= 1000) {
        return ((this.currentScore - 1000) / (10000 - 1000)) * 100;
      } else if (this.currentScore >= 500) {
        return ((this.currentScore - 500) / (1000 - 500)) * 100;
      } else {
        return (this.currentScore / 500) * 100;
      }
    }
  },
  methods: {
    // 领取代金券
    claimCoupon() {
      if (this.isCouponClaimed) return;
      this.isCouponClaimed = true;
      alert(`已领取${this.memberLevel.coupon}元代金券！`);
    },
    // 增加积分
    addScore(score) {
      this.currentScore += score;
      alert(`已增加${score}积分，当前积分：${this.currentScore}`);
    },
    // 重置积分
    resetScore() {
      this.currentScore = 0;
      this.isCouponClaimed = false;
      alert('积分已重置！');
    }
  }
}
</script>

<style scoped>
#member {
  min-height: 100vh;
  background-color: #f2f2f2;
}

.nav-bar {
  background-color: var(--color-tint);
  font-weight: 700;
  color: #fff;
}

/* 头部样式 */
.member-header {
  background-color: #fff;
  padding: 30px 20px;
  display: flex;
  align-items: center;
  margin-bottom: 15px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.member-avatar img {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  border: 3px solid rgba(255, 255, 255, 0.3);
}

.member-info {
  margin-left: 20px;
  flex: 1;
}

.member-name {
  margin: 0 0 10px 0;
  font-size: 20px;
  font-weight: 700;
  color: #333;
}

.member-level {
  display: inline-block;
  padding: 5px 15px;
  border-radius: 20px;
  font-size: 14px;
  margin-bottom: 15px;
}

.member-level.silver {
  background-color: rgba(192, 192, 192, 0.2);
  border: 1px solid #c0c0c0;
  color: #666;
}

.member-level.gold {
  background-color: rgba(255, 215, 0, 0.2);
  border: 1px solid #ffd700;
  color: #666;
}

.member-level.black-gold {
  background-color: rgba(0, 0, 0, 0.2);
  border: 1px solid #000;
  color: #fff;
  background-color: #333;
}

.member-level.normal {
  background-color: rgba(255, 129, 152, 0.2);
  border: 1px solid var(--color-tint);
  color: var(--color-tint);
}

.level-name {
  font-weight: 700;
  margin-right: 10px;
}

/* 进度条样式 */
.level-progress {
  width: 100%;
}

.progress-bar {
  width: 100%;
  height: 8px;
  background-color: #f0f0f0;
  border-radius: 4px;
  overflow: hidden;
  margin-bottom: 5px;
}

.progress-fill {
  height: 100%;
  background-color: var(--color-tint);
  transition: width 0.3s ease;
}

.progress-text {
  display: flex;
  justify-content: space-between;
  font-size: 12px;
  color: #999;
}

/* 通用区域样式 */
.section-title {
  font-size: 16px;
  font-weight: 700;
  margin: 20px 20px 15px;
  color: #333;
}

/* 福利区域样式 */
.member-benefits {
  background-color: #fff;
  margin: 0 10px 15px;
  border-radius: 5px;
  overflow: hidden;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.benefits-list {
  padding: 0 20px 20px;
}

.benefit-item {
  display: flex;
  align-items: center;
  padding: 20px 0;
  border-bottom: 1px solid #f0f0f0;
}

.benefit-item:last-child {
  border-bottom: none;
}

.benefit-icon {
  font-size: 32px;
  margin-right: 20px;
}

.benefit-info {
  flex: 1;
}

.benefit-info h4 {
  margin: 0 0 5px 0;
  color: #333;
  font-size: 16px;
}

.benefit-info p {
  margin: 0;
  color: #666;
  font-size: 14px;
}

.claim-btn {
  background-color: var(--color-tint);
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 20px;
  font-size: 14px;
  cursor: pointer;
}

.claim-btn:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

/* 等级说明样式 */
.member-levels {
  background-color: #fff;
  margin: 0 10px 15px;
  border-radius: 5px;
  overflow: hidden;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.levels-list {
  padding: 0;
}

.level-item-link {
  text-decoration: none;
  color: inherit;
  display: block;
}

.level-item {
  display: flex;
  align-items: center;
  padding: 15px 20px;
  border-bottom: 1px solid #f0f0f0;
  position: relative;
}

.level-item:last-child {
  border-bottom: none;
}

.level-item.active {
  background-color: rgba(255, 129, 152, 0.1);
}

.level-icon {
  font-size: 24px;
  margin-right: 20px;
}

.level-details {
  flex: 1;
}

.level-details h4 {
  margin: 0 0 5px 0;
  color: #333;
  font-size: 16px;
}

.level-details p {
  margin: 0;
  color: #666;
  font-size: 14px;
  margin-bottom: 3px;
}

.arrow-right {
  border-top: 1px solid #999;
  border-left: 1px solid #999;
  width: 9px;
  height: 9px;
  background-color: transparent;
  transform: rotate(135deg);
  display: inline-block;
  margin-left: .1rem;
}

/* 测试区域样式 */
.test-section {
  background-color: #fff;
  margin: 0 10px 15px;
  border-radius: 5px;
  overflow: hidden;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.test-buttons {
  padding: 0 20px 20px;
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}

.test-btn {
  background-color: var(--color-tint);
  color: white;
  border: none;
  padding: 10px 15px;
  border-radius: 5px;
  font-size: 14px;
  cursor: pointer;
  flex: 1;
  min-width: 120px;
}

.test-btn:active {
  background-color: #ff6b81;
}
</style>