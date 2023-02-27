<template>
  <view>
    <view class="container">
      <view class="date-selector">
        <text class="label">日期：</text>
        <picker mode="date" :value="date" @change="handleChange">
          <view class="picker-content">
            {{ formateDate(date) }}
          </view>
        </picker>
        <view class="button-group">
          <button class="button" type="primary" size="mini" @click="handleDateChange(-1)">上一天</button>
          <button class="button" type="primary" size="mini" @click="handleDateChange(1)">下一天</button>
        </view>
      </view>
	  
	  
      <view class="table-container">
        <view class="header-row">
          <view class="header-cell" v-for="(item, index) in header" :key="index">
            {{ item.name }}
          </view>
        </view>
        <scroll-view class="table-content" scroll-y :style="{ height: contentHeight + 'px' }">
          <view class="data-row" v-for="(row, rowIndex) in data" :key="rowIndex">
            <view class="data-cell" v-for="(cell, cellIndex) in row" :key="cellIndex">
              {{ cell }}
            </view>
          </view>
        </scroll-view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
		// 表头数据
		header: [
		  { "name": "姓名" },
		  { "name": "年龄" },
		  { "name": "性别" },
		  { "name": "地址" }
		],
		// 表格数据
		data: [
		  ["张三", 18, "男", "北京市"],
		  ["李四", 20, "女", "上海市"],
		  ["王五", 22, "男", "广州市"],
				["王五", 22, "男", "广州市"],
				["王五", 22, "男", "广州市"],
				["王五", 22, "男", "广州市"],
				["王五", 22, "男", "广州市"],
				["王五", 22, "男", "广州市"],
				["王五", 22, "男", "广州市"],
		  ["赵六", 24, "女", "深圳市"]
		],
		// 表格内容部分的高度
		contentHeight: 0,
		
      date: new Date().getTime()
    }
  },
  mounted() {
    // 计算表格内容部分的高度
    this.getContentHeight();
  },
  
  methods: {
    handleChange(e) {
      this.date = new Date(e.detail.value).getTime()
    },
    handleDateChange(offset) {
      const dayTimestamp = 24 * 60 * 60 * 1000 // 一天的毫秒数
      this.date = this.date + offset * dayTimestamp
    },
    formateDate(date) {
      const year = new Date(date).getFullYear()
      const month = new Date(date).getMonth() + 1
      const day = new Date(date).getDate()
      return `${year}-${month}-${day}`
    },
	
	getContentHeight() {
	  uni.createSelectorQuery()
	    .in(this)
	    .select('.table-content')
	    .boundingClientRect((rect) => {
	      this.contentHeight = rect.height;
	    })
	    .exec();
	}
	
	
  }
}
</script>

<style scoped>
.container {
  padding: 20px;
}

.date-selector {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.label {
  font-size: 16px;
}

.picker-content {
  font-size: 16px;
  color: #666;
  padding: 0 10px;
}

.button-group {
  display: flex;
 /* flex-direction: column; */
  margin-left: 10px;
}

.button {
  /* width: 40px;
  height: 40px; */
  font-size: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #00ff7f;
  color: #333;
  margin-bottom: 10px;
}


.table-container {
	top:100rpx;
  margin-bottom: 10px;
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
}

.header-row {
  display: flex;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  background-color: #fff;
  z-index: 1;
  /* 表头样式 */
  border-top: 1px solid #ccc;
  border-bottom: 1px solid #ccc;
}

.header-cell {
  flex: 1;
  padding: 10px;
  text-align: center;
  border-right: 1px solid #ccc;
}

.table-content {
  flex: 1;
}

.data-row {
  display: flex;
  /* 数据行样式 */
  border-bottom: 1px solid #ccc;
}

.data-cell {
  flex: 1;
  padding: 10px;
  text-align: center;
  border-right: 1px solid #ccc;
}
</style>
