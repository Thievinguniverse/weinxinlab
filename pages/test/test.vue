<template>
	<view>
		<uni-section title="意见反馈" subTitle="输入提出的意见" type="line" padding>
					<uni-easyinput type="textarea" autoHeight  v-model="value" placeholder="请输入内容" clearable="true"></uni-easyinput>
					<button @click="submit" type="primary" size="mini">提交</button>
				</uni-section>
	</view>
</template>
	

<script>
	export default {
	  data() {
	    return {
	      value: ''
	    }
	  },
	  methods: {
	    submit() {
	     uni.request({
	     				url: this.$Url + "/Applets/AddAdvice",
	     				 header: {
	     					'Content-Type': 'application/json'  
	     				}, 
	     			   method: 'POST',
	     			   data:{
	     			   	"advise": this.value,
	     			   	// "time": new Date().toLocaleTimeString()
	     			   },
	     				dataType:'json',
	     				success: (res) => {
								// const data = JSON.parse(res.data)
								// var obj = new Object();
								// obj = res
								
							    
	     			   }
						
	     			})
					
					console.log(this.value);
					console.log(this.$member.memberObj.id);
					this.value =""

	    }
	  }
	}
</script>

<style>

</style>
