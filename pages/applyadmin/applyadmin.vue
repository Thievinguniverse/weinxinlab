<template>
	<view>
		<!--   <view class="avatar">
      <image class="avatar-img" :src="user.avatarUrl"></image>
      <text class="avatar-name">{{user.nickName}}</text>
    </view> -->
		<!-- <view class="profile">
      <view class="profile-item">
        <text class="profile-label">用户ID</text>
        <text class="profile-value">{{user.id}}</text>
      </view>
     <view class="profile-item">
        <text class="profile-label">姓名</text>
        <text class="profile-value">{{user.name}}</text>
      </view>
      <view class="profile-item">
        <text class="profile-label">手机号</text>
        <text class="profile-value">{{user.phoneNumber}}</text>
      </view>
      <view class="profile-item">
        <text class="profile-label">邮箱</text>
        <text class="profile-value">{{user.email}}</text>
      </view>
    </view> -->
		<uni-section title="申请理由" subTitle="输入申请理由" type="line" padding>
			<uni-easyinput type="textarea" autoHeight v-model="value" placeholder="请输入内容" clearable="true">
			</uni-easyinput>
			<button class="edit-btn" type="primary" @click="applyadmin()">申请管理员</button>
		</uni-section>
		<!-- <button class="edit-btn" type="primary" @click="handleEdit()">编辑</button> -->
		<!-- <button class="edit-btn" type="primary" @click="applyadmin()">申请管理员</button> -->
	</view>
</template>

<script>
	export default {
		data() {
			return {
				value:'',
			}
		},
		methods: {

			applyadmin() {

				uni.request({
					url: this.$Url + "/Applets/Apply",
					header: {
						'Content-Type': 'application/json'
					},
					method: 'POST',
					data: {
						reason:this.value,
						name:this.$member.memberObj.id ,
						// "name": this.$member.memberObj.id,
						// "username": this.user.nickName,
						// "address": this.user.address,
						// "phone" : this.user.phoneNumber,
						// // "time": new Date().toLocaleTimeString()
					},
					dataType: 'json',

					success: (res) => {
						uni.hideLoading();
						if (res.data.msg === "ok") {
							uni.hideLoading()
							uni.showToast({
								title: '申请发送成功',
								icon: 'success'
							})

						}
						this.value = ''
						
					},

					
				})

			},
		}
	}
</script>

<style scoped>
	.container {
		display: flex;
		flex-direction: column;
		align-items: center;
		padding: 20rpx;
	}

	.avatar {
		display: flex;
		flex-direction: column;
		align-items: center;
		margin-bottom: 20rpx;
	}

	.avatar-img {
		width: 100rpx;
		height: 100rpx;
		border-radius: 50%;
	}

	.avatar-name {
		font-size: 28rpx;
		margin-top: 10rpx;
	}

	.profile {
		width: 100%;
		background-color: #fff;
		border-radius: 10rpx;
		padding: 20rpx;
		box-sizing: border-box;
	}

	.profile-item {
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: space-between;
		margin-bottom: 20rpx;
	}

	.profile-label {
		font-size: 28rpx;
		color: #666;
	}

	.profile-value {
		font-size: 28rpx;
		color: #333;
	}

	.edit-btn {
		width: 90%;
		height: 100rpx;
		margin-top: 40rpx;
		font-size: 32rpx;
		border-radius: 10rpx;
		background-color: #4caf50;
		color: #fff;
	}
</style>
