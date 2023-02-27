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
    </view>
		
  </view>
</template>

<script>
	export default {
		name:"showtime",
		data() {
		  return {
				
		    date: new Date().getTime()
		  }
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
  /* margin-bottom: 10px; */
}


</style>