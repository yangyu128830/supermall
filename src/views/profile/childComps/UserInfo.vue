<template>
  <div id="user-info">
    <div class="user-info-container clear-fix">
      <div class="user-avatar left">
        <slot name="user-icon">
          <svg class="avatar-svg">
            <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#avatar-default"></use>
          </svg>
        </slot>
      </div>
      <div class="user-details left">
        <div class="user-name">
          <slot name="user-nickname">
            <span class="login-prompt">登录/注册</span>
          </slot>
        </div>
        <div class="user-stats">
          <div class="stat-item">
            <span class="stat-number">0</span>
            <span class="stat-label">关注</span>
          </div>
          <div class="stat-item">
            <span class="stat-number">0</span>
            <span class="stat-label">粉丝</span>
          </div>
          <div class="stat-item">
            <span class="stat-number">0</span>
            <span class="stat-label">收藏</span>
          </div>
        </div>
      </div>
      <div class="user-actions right">
        <button class="action-btn">编辑资料</button>
        <svg fill="#fff" class="arrow-svg">
          <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#arrow-right"></use>
        </svg>
      </div>
    </div>
  </div>
</template>

<script>
	export default {
		name: "UserInfo"
	}
</script>

<style scoped>
  #user-info {
    background-color: var(--color-tint);
    padding: 20px 16px;
    margin-top: -5px;
  }

  .user-info-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .user-avatar .avatar-svg {
    width: 70px;
    height: 70px;
    background-color: #fff;
    border-radius: 35px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
  }

  .user-details {
    flex: 1;
    margin-left: 16px;
  }

  .user-name {
    font-size: 18px;
    font-weight: 700;
    color: #fff;
    margin-bottom: 8px;
  }

  .login-prompt {
    font-size: 16px;
    font-weight: 400;
  }

  .user-stats {
    display: flex;
  }

  .stat-item {
    margin-right: 24px;
    text-align: center;
  }

  .stat-number {
    display: block;
    font-size: 14px;
    font-weight: 700;
    color: #fff;
  }

  .stat-label {
    display: block;
    font-size: 12px;
    color: rgba(255, 255, 255, 0.8);
    margin-top: 2px;
  }

  .user-actions {
    display: flex;
    align-items: center;
  }

  .action-btn {
    background-color: rgba(255, 255, 255, 0.2);
    color: #fff;
    border: 1px solid rgba(255, 255, 255, 0.3);
    border-radius: 20px;
    padding: 6px 12px;
    font-size: 13px;
    margin-right: 8px;
    cursor: pointer;
    transition: all 0.3s ease;
  }

  .action-btn:hover {
    background-color: rgba(255, 255, 255, 0.3);
  }

  .arrow-svg {
    width: 12px;
    height: 24px;
  }

  .left {
    float: left;
  }

  .right {
    float: right;
  }
</style>
