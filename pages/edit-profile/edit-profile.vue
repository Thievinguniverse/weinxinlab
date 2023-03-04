<template>
	<view class="container">
		<view class="form-group">
			<text class="form-label">昵称</text>
			<input class="form-input" type="text" v-model="user.nickName" />
		</view>
		<!-- <view class="form-group">
      <text class="form-label">姓名</text>
      <input class="form-input" type="text" v-model="user.name" />
    </view> -->
		<view class="form-group">
			<text class="form-label">手机号</text>
			<input class="form-input" type="tel" v-model="user.phoneNumber" />
		</view>
		<view class="form-group">
			<text class="form-label">地址</text>
			<input class="form-input" type="email" v-model="user.address" />
		</view>
		<button class="submit-btn" type="primary" @click="handleSubmit">保存</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				user: {
					// avatarUrl: 'https://dummyimage.com/200x200/000/fff',
					nickName: '',
					// name: '',
					phoneNumber: '',
					email: ''
				}
			}
		},
		methods: {
			handleSubmit() {
				uni.showLoading({
					title: '提交中'
				})

				uni.request({
					url: this.$Url + "/Applets/UpdateUser",
					header: {
						'Content-Type': 'application/json'
					},
					method: 'POST',
					data: {
						"name": this.$member.memberObj.id,
						"username": this.user.nickName,
						"address": this.user.address,
						"phone" : this.user.phoneNumber,
						// "time": new Date().toLocaleTimeString()
					},
					dataType: 'json',
					success: (res) => {
						uni.hideLoading();
						if (res.data.msg === "ok") {
							setTimeout(() => {
							    uni.hideLoading()
							    uni.showToast({
							      title: '提交成功',
							      icon: 'success'
							    })
							    uni.navigateBack()
							  }, 2000)
							
						} else {
								uni.showToast({
									title: res.data.msg || '预约失败',
									icon: 'none',
								});
							}
						
					},
					
					fail: () => {
						uni.hideLoading();
						uni.showToast({
							title: '网络请求失败',
							icon: 'none',
						});
					
					},

				})

				// 将表单数据提交到后端服务器，这里假设请求成功后返回 { code: 0 }
				// 实际开发中需要根据具体情况进行处理
				// setTimeout(() => {
				// 	uni.hideLoading()
				// 	uni.showToast({
				// 		title: '提交成功',
				// 		icon: 'success'
				// 	})
				// 	uni.navigateBack("pages/index/index")
				// }, 2000)
			}
		}
	}
</script>

<style scoped>
	.container {
		padding: 20rpx;
	}

	.form-group {
		margin-bottom: 30rpx;
	}

	.form-label {
		font-size: 32rpx;
		color: #333;
		margin-bottom: 20rpx;
	}

	.form-input {
		width: 100%;
		height: 80rpx;
		font-size: 28rpx;
		border: 2rpx solid #e0e0e0;
		border-radius: 5rpx;
		padding: 0 20rpx;
		box-sizing: border-box;
		outline: none;
	}

	.submit-btn {
		width: 100%;
		height: 100rpx;
		font-size: 32rpx;
		border-radius: 10rpx;
		background-color: #4caf50;
		color: #fff;
		margin-top: 50rpx;
	}
</style>
