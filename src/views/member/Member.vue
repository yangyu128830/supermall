<template>
  <div class="member-container">
    <!-- 会员信息头部 -->
    <div class="member-header" :class="memberLevel.class">
      <div class="header-decoration">
        <div class="decoration-circle decoration-circle-1"></div>
        <div class="decoration-circle decoration-circle-2"></div>
        <div class="decoration-circle decoration-circle-3"></div>
      </div>
      <div class="member-info">
        <div class="member-avatar">
          <img src="~assets/img/tabbar/profile.svg" alt="会员头像">
          <div class="avatar-badge" :class="memberLevel.class">{{ memberLevel.name.charAt(0) }}</div>
        </div>
        <div class="member-details">
          <h2 class="member-name">尊敬的会员</h2>
          <div class="member-level-info">
            <span class="member-level" :class="memberLevel.class">{{ memberLevel.name }}</span>
            <span class="member-points">
              <svg class="points-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M12 2L15.09 8.26L22 9.27L17 14.14L18.18 21.02L12 17.77L5.82 21.02L7 14.14L2 9.27L8.91 8.26L12 2Z"/>
              </svg>
              {{ currentPoints }} 积分
            </span>
          </div>
          <div class="level-progress">
            <div class="progress-bar">
              <div class="progress-fill" :class="memberLevel.class" :style="{ width: progressWidth + '%' }"></div>
            </div>
            <div class="progress-text">
              <span class="current-progress">{{ currentPoints }}/{{ nextLevelPoints }}</span>
              <span class="next-level-info">距离{{ nextLevelName }}还差 {{ nextLevelPoints - currentPoints }} 积分</span>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- 会员福利区域 -->
    <div class="benefits-section">
      <div class="section-header">
        <h3 class="section-title">本月福利</h3>
        <span class="section-subtitle">专属于您的会员权益</span>
      </div>
      <div class="benefit-card" :class="memberLevel.class">
        <div class="benefit-icon">
          <svg class="coupon-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <rect x="2" y="5" width="20" height="14" rx="2"/>
            <line x1="7" y1="10" x2="17" y2="10"/>
            <line x1="7" y1="14" x2="17" y2="14"/>
            <circle cx="6" cy="12" r="1"/>
            <circle cx="18" cy="12" r="1"/>
          </svg>
        </div>
        <div class="benefit-info">
          <h4 class="benefit-title">月度代金券</h4>
          <p class="benefit-value">价值 <span class="amount">{{ memberLevel.couponAmount }}</span> 元</p>
          <p class="benefit-desc">每月1号可领取，有效期30天</p>
        </div>
        <button class="claim-btn" :class="{ 'claimed': couponClaimed }" @click="claimCoupon" :disabled="couponClaimed">
          <span v-if="!couponClaimed">
            <svg class="claim-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M9 11l3 3L22 4"/>
              <path d="M21 12v7a2 2 0 01-2 2H5a2 2 0 01-2-2V5a2 2 0 012-2h11"/>
            </svg>
            立即领取
          </span>
          <span v-else>
            <svg class="claimed-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M9 11l3 3L22 4"/>
              <path d="M21 12v7a2 2 0 01-2 2H5a2 2 0 01-2-2V5a2 2 0 012-2h11"/>
            </svg>
            已领取
          </span>
        </button>
      </div>
    </div>

    <!-- 会员等级说明 -->
    <div class="levels-section">
      <div class="section-header">
        <h3 class="section-title">会员等级</h3>
        <span class="section-subtitle">解锁更多专属权益</span>
      </div>
      <div class="levels-list">
        <div class="level-item" :class="{ active: level.name === memberLevel.name }" v-for="level in levels" :key="level.name">
          <div class="level-header">
            <div class="level-info">
              <span class="level-name" :class="level.class">{{ level.name }}</span>
              <span class="level-requirement">{{ level.points }} 积分</span>
            </div>
            <div class="level-badge" :class="level.class">{{ level.name.charAt(0) }}</div>
          </div>
          <div class="level-benefits">
            <div class="benefit-item">
              <svg class="benefit-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <rect x="2" y="5" width="20" height="14" rx="2"/>
                <line x1="7" y1="10" x2="17" y2="10"/>
                <line x1="7" y1="14" x2="17" y2="14"/>
              </svg>
              <span>每月可领取 {{ level.couponAmount }} 元代金券</span>
            </div>
          </div>
          <div class="level-progress-mini" v-if="level.name !== memberLevel.name && memberLevel.points < level.points">
            <div class="progress-bar-mini">
              <div class="progress-fill-mini" :style="{ width: Math.min((currentPoints / level.points) * 100, 100) + '%' }"></div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- 测试操作区域 -->
    <div class="test-section">
      <div class="section-header">
        <h3 class="section-title">测试功能</h3>
        <span class="section-subtitle">快速体验不同等级权益</span>
      </div>
      <div class="test-buttons">
        <button class="test-btn add-btn" @click="addPoints(100)">
          <svg class="test-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <line x1="12" y1="5" x2="12" y2="19"/>
            <line x1="5" y1="12" x2="19" y2="12"/>
          </svg>
          增加100积分
        </button>
        <button class="test-btn add-btn" @click="addPoints(500)">
          <svg class="test-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <line x1="12" y1="5" x2="12" y2="19"/>
            <line x1="5" y1="12" x2="19" y2="12"/>
          </svg>
          增加500积分
        </button>
        <button class="test-btn reset-btn" @click="resetPoints">
          <svg class="test-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <polyline points="1 4 1 10 7 10"/>
            <path d="M3.51 15a9 9 0 102.13-9.36L1 10"/>
          </svg>
          重置积分
        </button>
      </div>
    </div>

    <!-- 领取成功提示 -->
    <div class="toast" v-if="showToast">
      <div class="toast-content">
        <svg class="toast-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M9 11l3 3L22 4"/>
          <path d="M21 12v7a2 2 0 01-2 2H5a2 2 0 01-2-2V5a2 2 0 012-2h11"/>
        </svg>
        <span>{{ toastMessage }}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Member',
  data() {
    return {
      currentPoints: 800, // 当前积分
      couponClaimed: false, // 代金券是否已领取
      showToast: false, // 是否显示提示
      toastMessage: '', // 提示消息
      levels: [
        {
          name: '白银会员',
          points: 500,
          couponAmount: 50,
          class: 'silver'
        },
        {
          name: '黄金会员',
          points: 1000,
          couponAmount: 100,
          class: 'gold'
        },
        {
          name: '黑金会员',
          points: 10000,
          couponAmount: 200,
          class: 'black-gold'
        }
      ]
    }
  },
  computed: {
    // 当前会员等级
    memberLevel() {
      if (this.currentPoints >= 10000) {
        return this.levels[2]
      } else if (this.currentPoints >= 1000) {
        return this.levels[1]
      } else if (this.currentPoints >= 500) {
        return this.levels[0]
      } else {
        return {
          name: '普通会员',
          points: 0,
          couponAmount: 0,
          class: 'normal'
        }
      }
    },
    // 下一级需要的积分
    nextLevelPoints() {
      if (this.currentPoints < 500) {
        return 500
      } else if (this.currentPoints < 1000) {
        return 1000
      } else if (this.currentPoints < 10000) {
        return 10000
      } else {
        return this.currentPoints // 已是最高等级
      }
    },
    // 下一级名称
    nextLevelName() {
      if (this.currentPoints < 500) {
        return '白银会员'
      } else if (this.currentPoints < 1000) {
        return '黄金会员'
      } else if (this.currentPoints < 10000) {
        return '黑金会员'
      } else {
        return '最高等级'
      }
    },
    // 进度条宽度
    progressWidth() {
      if (this.currentPoints >= 10000) {
        return 100
      } else if (this.currentPoints >= 1000) {
        return ((this.currentPoints - 1000) / (10000 - 1000)) * 100
      } else if (this.currentPoints >= 500) {
        return ((this.currentPoints - 500) / (1000 - 500)) * 100
      } else {
        return (this.currentPoints / 500) * 100
      }
    }
  },
  methods: {
    // 领取代金券
    claimCoupon() {
      if (this.memberLevel.couponAmount === 0) {
        this.showToastMessage('您当前等级暂无代金券可领取')
        return
      }
      
      this.couponClaimed = true
      this.showToastMessage(`恭喜您领取了${this.memberLevel.couponAmount}元代金券！`)
    },
    // 增加积分
    addPoints(amount) {
      this.currentPoints += amount
      this.showToastMessage(`增加了${amount}积分！`)
    },
    // 重置积分
    resetPoints() {
      this.currentPoints = 0
      this.couponClaimed = false
      this.showToastMessage('积分已重置！')
    },
    // 显示提示消息
    showToastMessage(message) {
      this.toastMessage = message
      this.showToast = true
      setTimeout(() => {
        this.showToast = false
      }, 3000)
    }
  }
}
</script>

<style scoped>
.member-container {
  min-height: 100vh;
  background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
  padding-bottom: 20px;
}

/* 会员头部样式 */
.member-header {
  padding: 40px 20px 30px;
  color: white;
  position: relative;
  overflow: hidden;
  border-radius: 0 0 30px 30px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
}

.header-decoration {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  overflow: hidden;
}

.decoration-circle {
  position: absolute;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.1);
  animation: float 6s ease-in-out infinite;
}

.decoration-circle-1 {
  width: 100px;
  height: 100px;
  top: 20px;
  right: 30px;
  animation-delay: 0s;
}

.decoration-circle-2 {
  width: 60px;
  height: 60px;
  top: 80px;
  right: 100px;
  animation-delay: 2s;
}

.decoration-circle-3 {
  width: 80px;
  height: 80px;
  bottom: 30px;
  right: 50px;
  animation-delay: 4s;
}

@keyframes float {
  0%, 100% {
    transform: translateY(0) scale(1);
  }
  50% {
    transform: translateY(-20px) scale(1.1);
  }
}

.member-header.silver {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}

.member-header.gold {
  background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
  color: white;
}

.member-header.black-gold {
  background: linear-gradient(135deg, #434343 0%, #000000 100%);
}

.member-header.normal {
  background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
}

.member-info {
  display: flex;
  align-items: center;
  position: relative;
  z-index: 1;
}

.member-avatar {
  width: 90px;
  height: 90px;
  border-radius: 50%;
  overflow: hidden;
  border: 4px solid rgba(255, 255, 255, 0.3);
  margin-right: 20px;
  position: relative;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
}

.member-avatar img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.avatar-badge {
  position: absolute;
  bottom: -5px;
  right: -5px;
  width: 30px;
  height: 30px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 14px;
  font-weight: bold;
  color: white;
  border: 3px solid white;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
}

.avatar-badge.silver {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}

.avatar-badge.gold {
  background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
}

.avatar-badge.black-gold {
  background: linear-gradient(135deg, #434343 0%, #000000 100%);
}

.avatar-badge.normal {
  background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
}

.member-details {
  flex: 1;
}

.member-name {
  font-size: 26px;
  font-weight: bold;
  margin-bottom: 12px;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.member-level-info {
  display: flex;
  align-items: center;
  margin-bottom: 18px;
  flex-wrap: wrap;
}

.member-level {
  font-size: 18px;
  font-weight: bold;
  margin-right: 15px;
  padding: 6px 16px;
  border-radius: 25px;
  background: rgba(255, 255, 255, 0.2);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.3);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.member-points {
  display: flex;
  align-items: center;
  font-size: 16px;
  opacity: 0.9;
  background: rgba(255, 255, 255, 0.15);
  padding: 4px 12px;
  border-radius: 20px;
  backdrop-filter: blur(10px);
}

.points-icon {
  width: 16px;
  height: 16px;
  margin-right: 6px;
  fill: #ffd700;
}

.level-progress {
  margin-top: 20px;
}

.progress-bar {
  width: 100%;
  height: 10px;
  background: rgba(255, 255, 255, 0.3);
  border-radius: 10px;
  overflow: hidden;
  margin-bottom: 10px;
  box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.1);
}

.progress-fill {
  height: 100%;
  border-radius: 10px;
  transition: width 0.5s ease;
  position: relative;
  overflow: hidden;
}

.progress-fill::after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.3), transparent);
  animation: shimmer 2s infinite;
}

@keyframes shimmer {
  0% {
    transform: translateX(-100%);
  }
  100% {
    transform: translateX(100%);
  }
}

.progress-fill.silver {
  background: linear-gradient(90deg, #667eea 0%, #764ba2 100%);
}

.progress-fill.gold {
  background: linear-gradient(90deg, #f093fb 0%, #f5576c 100%);
}

.progress-fill.black-gold {
  background: linear-gradient(90deg, #434343 0%, #000000 100%);
}

.progress-fill.normal {
  background: linear-gradient(90deg, #4facfe 0%, #00f2fe 100%);
}

.progress-text {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 13px;
  opacity: 0.95;
}

.current-progress {
  font-weight: bold;
  background: rgba(255, 255, 255, 0.2);
  padding: 2px 8px;
  border-radius: 12px;
  backdrop-filter: blur(10px);
}

.next-level-info {
  font-size: 12px;
}

/* 通用区域样式 */
.benefits-section,
.levels-section,
.test-section {
  padding: 0 20px 20px;
  margin-top: 20px;
}

.section-header {
  margin-bottom: 20px;
}

.section-title {
  font-size: 20px;
  font-weight: bold;
  color: #333;
  margin-bottom: 5px;
  display: flex;
  align-items: center;
}

.section-title::before {
  content: '';
  width: 4px;
  height: 20px;
  background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
  border-radius: 2px;
  margin-right: 10px;
}

.section-subtitle {
  font-size: 14px;
  color: #666;
  margin-left: 14px;
}

/* 福利区域样式 */
.benefit-card {
  background: white;
  border-radius: 20px;
  padding: 25px;
  display: flex;
  align-items: center;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
  position: relative;
  overflow: hidden;
  transition: all 0.3s ease;
}

.benefit-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 15px 40px rgba(0, 0, 0, 0.15);
}

.benefit-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  width: 6px;
}

.benefit-card.silver::before {
  background: linear-gradient(180deg, #667eea 0%, #764ba2 100%);
}

.benefit-card.gold::before {
  background: linear-gradient(180deg, #f093fb 0%, #f5576c 100%);
}

.benefit-card.black-gold::before {
  background: linear-gradient(180deg, #434343 0%, #000000 100%);
}

.benefit-icon {
  width: 70px;
  height: 70px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 20px;
  position: relative;
  overflow: hidden;
}

.benefit-card.silver .benefit-icon {
  background: linear-gradient(135deg, rgba(102, 126, 234, 0.1) 0%, rgba(118, 75, 162, 0.1) 100%);
}

.benefit-card.gold .benefit-icon {
  background: linear-gradient(135deg, rgba(240, 147, 251, 0.1) 0%, rgba(245, 87, 108, 0.1) 100%);
}

.benefit-card.black-gold .benefit-icon {
  background: linear-gradient(135deg, rgba(67, 67, 67, 0.1) 0%, rgba(0, 0, 0, 0.1) 100%);
}

.coupon-icon {
  width: 35px;
  height: 35px;
  color: #3CABFF;
}

.benefit-info {
  flex: 1;
}

.benefit-title {
  font-size: 18px;
  font-weight: bold;
  color: #333;
  margin-bottom: 8px;
}

.benefit-value {
  font-size: 16px;
  color: #666;
  margin-bottom: 5px;
}

.amount {
  font-size: 24px;
  font-weight: bold;
  color: #3CABFF;
}

.benefit-desc {
  font-size: 13px;
  color: #999;
}

.claim-btn {
  padding: 12px 24px;
  border: none;
  border-radius: 25px;
  font-size: 14px;
  font-weight: bold;
  color: white;
  background: linear-gradient(135deg, #3CABFF 0%, #2196F3 100%);
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  gap: 8px;
  box-shadow: 0 4px 15px rgba(60, 171, 255, 0.3);
}

.claim-btn:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(60, 171, 255, 0.4);
}

.claim-btn.claimed {
  background: linear-gradient(135deg, #ccc 0%, #999 100%);
  cursor: not-allowed;
  transform: none;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.claim-icon,
.claimed-icon {
  width: 16px;
  height: 16px;
}

/* 会员等级说明样式 */
.levels-list {
  background: white;
  border-radius: 20px;
  overflow: hidden;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
}

.level-item {
  padding: 20px;
  border-bottom: 1px solid #f0f0f0;
  transition: all 0.3s ease;
  position: relative;
}

.level-item:last-child {
  border-bottom: none;
}

.level-item:hover {
  background: #fafafa;
}

.level-item.active {
  background: linear-gradient(135deg, rgba(60, 171, 255, 0.05) 0%, rgba(33, 150, 243, 0.05) 100%);
  border-left: 5px solid #3CABFF;
}

.level-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 12px;
}

.level-info {
  display: flex;
  align-items: center;
  gap: 12px;
}

.level-name {
  font-size: 17px;
  font-weight: bold;
  color: #333;
  position: relative;
}

.level-name::after {
  content: '';
  position: absolute;
  bottom: -3px;
  left: 0;
  width: 0;
  height: 2px;
  transition: width 0.3s ease;
}

.level-item:hover .level-name::after {
  width: 100%;
}

.level-name.silver::after {
  background: linear-gradient(90deg, #667eea 0%, #764ba2 100%);
}

.level-name.gold::after {
  background: linear-gradient(90deg, #f093fb 0%, #f5576c 100%);
}

.level-name.black-gold::after {
  background: linear-gradient(90deg, #434343 0%, #000000 100%);
}

.level-requirement {
  font-size: 14px;
  color: #666;
  background: #f0f0f0;
  padding: 4px 12px;
  border-radius: 15px;
  font-weight: 500;
}

.level-badge {
  width: 35px;
  height: 35px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 16px;
  font-weight: bold;
  color: white;
  box-shadow: 0 3px 10px rgba(0, 0, 0, 0.2);
}

.level-badge.silver {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}

.level-badge.gold {
  background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
}

.level-badge.black-gold {
  background: linear-gradient(135deg, #434343 0%, #000000 100%);
}

.level-benefits {
  margin-bottom: 10px;
}

.benefit-item {
  display: flex;
  align-items: center;
  gap: 10px;
  font-size: 14px;
  color: #666;
}

.benefit-item .benefit-icon {
  width: 20px;
  height: 20px;
  border-radius: 0;
  background: none;
  margin: 0;
}

.benefit-item .benefit-icon svg {
  width: 18px;
  height: 18px;
  color: #3CABFF;
}

.level-progress-mini {
  margin-top: 10px;
}

.progress-bar-mini {
  width: 100%;
  height: 6px;
  background: #f0f0f0;
  border-radius: 3px;
  overflow: hidden;
}

.progress-fill-mini {
  height: 100%;
  background: linear-gradient(90deg, #4facfe 0%, #00f2fe 100%);
  border-radius: 3px;
  transition: width 0.5s ease;
}

/* 测试操作区域样式 */
.test-buttons {
  display: flex;
  gap: 12px;
  flex-wrap: wrap;
}

.test-btn {
  padding: 12px 20px;
  border: none;
  border-radius: 25px;
  font-size: 14px;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  gap: 8px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
}

.test-icon {
  width: 16px;
  height: 16px;
}

.add-btn {
  background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
  color: white;
}

.add-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(79, 172, 254, 0.4);
}

.reset-btn {
  background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
  color: white;
}

.reset-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(245, 87, 108, 0.4);
}

/* 提示消息样式 */
.toast {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 1000;
  animation: fadeInOut 3s ease;
}

.toast-content {
  background: rgba(0, 0, 0, 0.85);
  color: white;
  padding: 18px 28px;
  border-radius: 12px;
  font-size: 14px;
  display: flex;
  align-items: center;
  gap: 10px;
  backdrop-filter: blur(10px);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.toast-icon {
  width: 20px;
  height: 20px;
  fill: #4CAF50;
}

@keyframes fadeInOut {
  0% {
    opacity: 0;
    transform: translate(-50%, -50%) translateY(20px) scale(0.9);
  }
  15% {
    opacity: 1;
    transform: translate(-50%, -50%) translateY(0) scale(1);
  }
  85% {
    opacity: 1;
    transform: translate(-50%, -50%) translateY(0) scale(1);
  }
  100% {
    opacity: 0;
    transform: translate(-50%, -50%) translateY(20px) scale(0.9);
  }
}
</style>