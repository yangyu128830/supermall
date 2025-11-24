<template>
    <div id="list">
      <div v-for="(item, index) in listData" :key="index" class="item clear-fix">
        <div class="item-left left">
          <span class="icon">
            <svg :fill="item.iconColor"><use :xlink:href="item.icon"></use></svg>
          </span>
          <div class="info">{{item.info}}</div>
        </div>
        <div class="item-right right">
          <div v-if="item.badge" class="badge">{{item.badge}}</div>
          <span class="arrow-right"></span>
        </div>
      </div>
    </div>
</template>

<script>
	export default {
		name: "ListView",
    props: {
		  listData: {
		    type: Array,
        default: function () {
          return []
        }
      }
    }
	}
</script>

<style scoped>
  #list {
    background-color: #fff;
    font-size: 15px;
    color: #333;
  }

  #list .item {
    height: 48px;
    line-height: 48px;
    position: relative;
    padding: 0 16px;
  }

  .item-left {
    display: flex;
    align-items: center;
  }

  .icon svg {
    width: 20px;
    height: 20px;
    margin-right: 12px;
  }

  .item .info {
    font-size: 15px;
    color: #333;
  }

  .item-right {
    display: flex;
    align-items: center;
    color: #999;
  }

  .badge {
    background-color: #ff5777;
    color: #fff;
    font-size: 12px;
    padding: 2px 6px;
    border-radius: 10px;
    margin-right: 8px;
    min-width: 18px;
    text-align: center;
  }

  .arrow-right {
    border-top: 1px solid #999;
    border-left: 1px solid #999;
    width: 9px;
    height: 9px;
    background-color: transparent;
    transform: rotate(135deg);
    display: inline-block;
  }

  .left {
    float: left;
  }

  .right {
    float: right;
  }
</style>
