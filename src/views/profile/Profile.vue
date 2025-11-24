<template>
  <div id="profile">
    <nav-bar class="nav-bar"><div slot="center">会员中心</div></nav-bar>
    <!-- 用户信息区域 -->
    <UserInfo></UserInfo>

    <!-- 订单状态区域 -->
    <section class="order-status">
      <div class="order-header">
        <div class="title">我的订单</div>
        <div class="more">查看全部 <span class="arrow-right"></span></div>
      </div>
      <div class="order-types">
        <div class="order-type" v-for="(type, index) in orderTypes" :key="index">
          <svg :fill="type.color" class="icon">
            <use :xlink:href="type.icon"></use>
          </svg>
          <div class="text">{{type.text}}</div>
        </div>
      </div>
    </section>

    <!-- 账户信息区域 -->
    <section class="account">
      <div class="account-item">
        <div class="number">
          <span class="balance">0.00</span>元
        </div>
        <div class="account-info">我的余额</div>
      </div>
      <div class="account-item">
        <div class="number">
          <span class="balance">0</span>个
        </div>
        <div class="account-info">我的优惠券</div>
      </div>
      <div class="account-item">
        <div class="number">
          <span class="balance">0</span>分
        </div>
        <div class="account-info">我的积分</div>
      </div>
    </section>

    <!-- 功能列表区域 -->
    <list-view :list-data="functionList" class="function-list"></list-view>
    <list-view :list-data="serviceList" class="service-list"></list-view>
  </div>
</template>

<script>
  import UserInfo from './childComps/UserInfo'
  import ListView from './childComps/ListView'
  import NavBar from 'common/navbar/NavBar'

	export default {
		name: "Profile",
    components: {
		  UserInfo, ListView, NavBar
    },
    data: function () {
		  return {
        orderTypes: [
          {icon: '#order-all', color: '#ff8198', text: '全部'},
          {icon: '#order-pending', color: '#fc7b53', text: '待付款'},
          {icon: '#order-shipping', color: '#ffc636', text: '待发货'},
          {icon: '#order-receiving', color: '#4b8fed', text: '待收货'},
          {icon: '#order-evaluating', color: '#37c2bc', text: '待评价'},
        ],
        functionList: [
          {icon: '#message', iconColor: '#ff8198', info: '我的消息'},
          {icon: '#point', iconColor: '#fc7b53', info: '积分商城'},
          {icon: '#vip', iconColor: '#ffc636', info: '会员卡'},
          {icon: '#address', iconColor: '#4b8fed', info: '收货地址'},
          {icon: '#collect', iconColor: '#37c2bc', info: '我的收藏'},
          {icon: '#footprint', iconColor: '#9c27b0', info: '浏览足迹'},
        ],
        serviceList: [
          {icon: '#service', iconColor: '#ff8198', info: '客户服务'},
          {icon: '#settings', iconColor: '#666', info: '设置'},
        ]
      }
    },
    mounted: function () {
    }
	}
</script>

<style scoped>
  #profile {
    background-color: #f2f2f2;
    min-height: 100vh;
  }

  .nav-bar {
    background-color: var(--color-tint);
    font-weight: 700;
    color: #fff;
  }

  /* 订单状态区域 */
  .order-status {
    background-color: #fff;
    margin-top: 12px;
  }

  .order-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 12px 16px;
    border-bottom: 1px solid rgba(100, 100, 100, .1);
  }

  .order-header .title {
    font-size: 15px;
    font-weight: 700;
    color: #333;
  }

  .order-header .more {
    font-size: 13px;
    color: #999;
  }

  .order-types {
    display: flex;
    padding: 16px 0;
  }

  .order-type {
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .order-type .icon {
    width: 28px;
    height: 28px;
    margin-bottom: 6px;
  }

  .order-type .text {
    font-size: 13px;
    color: #666;
  }

  /* 账户信息区域 */
  .account {
    display: flex;
    background-color: #fff;
    margin-top: 12px;
  }

  .account-item {
    flex: 1;
    text-align: center;
    padding: 18px;
    border-right: 1px solid rgba(100, 100, 100, .1);
  }

  .account-item:last-of-type {
    border-right: none;
  }

  .account-item .number {
    font-size: 24px;
    font-weight: 700;
    color: #ff5f3e;
  }

  .account-item .account-info {
    margin-top: 6px;
    font-size: 13px;
    color: #666;
  }

  /* 功能列表区域 */
  .function-list, .service-list {
    margin-top: 12px;
  }

</style>
