<template>
  <div id="level-detail">
    <nav-bar class="nav-bar">
      <div slot="left">
        <router-link to="/member" class="back-link">
          <span class="arrow-left">←</span>
        </router-link>
      </div>
      <div slot="center">{{ level.name }}详情</div>
    </nav-bar>

    <!-- 等级信息 -->
    <div class="level-info">
      <div class="level-icon">{{ level.icon }}</div>
      <h2 class="level-name">{{ level.name }}</h2>
      <div class="level-requirement">
        <span>所需积分：{{ level.needScore }}分</span>
      </div>
    </div>

    <!-- 等级福利 -->
    <div class="level-benefits">
      <h3 class="section-title">会员福利</h3>
      <div class="benefits-list">
        <div class="benefit-item">
          <div class="benefit-icon">🎁</div>
          <div class="benefit-info">
            <h4>每月代金券</h4>
            <p>{{ level.coupon }}元</p>
          </div>
        </div>
        <div class="benefit-item">
          <div class="benefit-icon">🎟️</div>
          <div class="benefit-info">
            <h4>专属活动</h4>
            <p>定期举办{{ level.name }}专属优惠活动</p>
          </div>
        </div>
        <div class="benefit-item">
          <div class="benefit-icon">🎊</div>
          <div class="benefit-info">
            <h4>生日福利</h4>
            <p>生日当月可领取专属生日礼品</p>
          </div>
        </div>
        <div class="benefit-item">
          <div class="benefit-icon">🚀</div>
          <div class="benefit-info">
            <h4>优先服务</h4>
            <p>享受客服优先响应和物流优先配送</p>
          </div>
        </div>
      </div>
    </div>

    <!-- 升级指南 -->
    <div class="upgrade-guide">
      <h3 class="section-title">升级指南</h3>
      <div class="guide-content">
        <p>当前积分：{{ currentScore }}分</p>
        <p v-if="currentScore < level.needScore">
          还需{{ level.needScore - currentScore }}分即可升级为{{ level.name }}
        </p>
        <p v-else>
          您已达到{{ level.name }}等级要求
        </p>
        <p>积分获取方式：</p>
        <ul>
          <li>购物消费：每消费1元获得1积分</li>
          <li>参与活动：参与平台活动获得额外积分</li>
          <li>邀请好友：邀请好友注册并消费获得积分奖励</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import NavBar from 'common/navbar/NavBar'

export default {
  name: 'LevelDetail',
  components: {
    NavBar
  },
  data() {
    return {
      currentScore: 800, // 默认积分，实际应用中可从Vuex或后端获取
      allLevels: [
        { name: '白银会员', needScore: 500, coupon: 50, class: 'silver', icon: '🥈' },
        { name: '黄金会员', needScore: 1000, coupon: 100, class: 'gold', icon: '🥇' },
        { name: '黑金会员', needScore: 10000, coupon: 200, class: 'black-gold', icon: '💎' }
      ]
    }
  },
  computed: {
    level() {
      const levelName = this.$route.params.levelName
      return this.allLevels.find(level => level.name === levelName) || this.allLevels[0]
    }
  }
}
</script>

<style scoped>
#level-detail {
  min-height: 100vh;
  background-color: #f2f2f2;
}

.nav-bar {
  background-color: var(--color-tint);
  font-weight: 700;
  color: #fff;
  position: relative;
}

.back-link {
  color: #fff;
  text-decoration: none;
  display: flex;
  align-items: center;
}

.arrow-left {
  font-size: 20px;
  margin-right: 5px;
}

/* 等级信息样式 */
.level-info {
  background-color: #fff;
  padding: 30px 20px;
  text-align: center;
  margin-bottom: 15px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.level-icon {
  font-size: 64px;
  margin-bottom: 15px;
}

.level-name {
  margin: 0 0 10px 0;
  font-size: 24px;
  font-weight: 700;
  color: #333;
}

.level-requirement {
  font-size: 16px;
  color: #666;
}

/* 通用区域样式 */
.section-title {
  font-size: 16px;
  font-weight: 700;
  margin: 20px 20px 15px;
  color: #333;
}

/* 福利区域样式 */
.level-benefits {
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
  padding: 15px 0;
  border-bottom: 1px solid #f0f0f0;
}

.benefit-item:last-child {
  border-bottom: none;
}

.benefit-icon {
  font-size: 28px;
  margin-right: 20px;
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

/* 升级指南样式 */
.upgrade-guide {
  background-color: #fff;
  margin: 0 10px 15px;
  border-radius: 5px;
  overflow: hidden;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.guide-content {
  padding: 0 20px 20px;
}

.guide-content p {
  margin: 0 0 10px 0;
  color: #666;
  font-size: 14px;
}

.guide-content ul {
  margin: 10px 0;
  padding-left: 20px;
}

.guide-content li {
  color: #666;
  font-size: 14px;
  margin-bottom: 5px;
}
</style>