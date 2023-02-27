<template>
  <view>
    <picker 
      mode="selector" 
      :range="options" 
      @change="onSelect"
    >
      <view class="picker">
		  {{ selected }}
      </view>
    </picker>
	
	<uni-section title="申请理由" subTitle="申请理由" type="line" padding>
				<uni-easyinput type="textarea" autoHeight  v-model="value" placeholder="请输入内容" clearable="true"></uni-easyinput>
				<!-- <button @click="submit" type="primary" size="mini">提交</button> -->
	</uni-section>
  </view>
</template>

<script>
	export default {
		name:"Mypicker",
		 data() {
		    return {
		      options: ['8:00-10:00', '10:15-12:15', '14:00-16:00', '16:15-18:15'],
		      selected: '请选择预约时间段'
		    }
		  },
		  methods: {
		    onSelect(event) {
		      const { value } = event.detail
		      this.selected = this.options[value]
		    }
		  }
	}
</script>

<style>

</style>