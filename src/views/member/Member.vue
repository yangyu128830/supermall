<template>
  <div id="member">
    <div class="member-header">
      <div class="member-info">
        <div class="member-avatar">
          <svg class="avatar-svg">
            <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#avatar-default"></use>
          </svg>
        </div>
        <div class="member-details">
          <div class="member-name">登录/注册</div>
          <div class="member-level">
            <span class="level-tag">{{ memberLevel.name }}</span>
            <span class="level-desc">{{ memberLevel.desc }}</span>
          </div>
        </div>
      </div>
      <div class="member-points">
        <div class="points-label">当前积分</div>
        <div class="points-value">{{ currentPoints }}</div>
      </div>
    </div>
    
    <div class="member-benefits">
      <h3>会员福利</h3>
      <div class="benefits-list">
        <div class="benefit-item">
          <div class="benefit-icon">
            <svg class="icon">
              <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#point"></use>
            </svg>
          </div>
          <div class="benefit-content">
            <div class="benefit-title">每月代金券</div>
            <div class="benefit-desc">每月可领取{{ memberLevel.coupon }}元代金券</div>
          </div>
          <div class="benefit-action">
            <button class="claim-btn" @click="claimCoupon">立即领取</button>
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
</template>

<script>
export default {
  name: "Member",
  data() {
    return {
      currentPoints: 800,
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
    }
  }
};
</script>

<style scoped>
#member {
  padding-bottom: 50px;
}

.member-header {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: #fff;
  padding: 20px;
  text-align: center;
}

.member-info {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 20px;
}

.member-avatar {
  margin-right: 20px;
}

.avatar-svg {
  width: 80px;
  height: 80px;
  background-color: rgba(255, 255, 255, 0.3);
  border-radius: 50%;
}

.member-details {
  text-align: left;
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
  font-size: 16px;
}

.points-label {
  margin-bottom: 5px;
}

.points-value {
  font-size: 24px;
  font-weight: bold;
}

.member-benefits,
.member-levels {
  padding: 20px;
  margin-bottom: 20px;
  background-color: #fff;
  border-radius: 10px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.member-benefits h3,
.member-levels h3 {
  margin-bottom: 15px;
  font-size: 16px;
  font-weight: bold;
  color: #333;
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
  border-bottom: 1px solid #eee;
}

.benefit-item:last-child {
  border-bottom: none;
}

.benefit-icon {
  margin-right: 15px;
}

.benefit-icon .icon {
  width: 40px;
  height: 40px;
  fill: #667eea;
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
  background-color: #667eea;
  color: #fff;
  border: none;
  padding: 8px 16px;
  border-radius: 20px;
  font-size: 12px;
  cursor: pointer;
}

.level-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 15px 0;
  border-bottom: 1px solid #eee;
  position: relative;
}

.level-item:last-child {
  border-bottom: none;
}

.level-item.active {
  color: #667eea;
  font-weight: bold;
}

.level-item.active::after {
  content: "当前等级";
  position: absolute;
  top: 10px;
  right: 0;
  background-color: #667eea;
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