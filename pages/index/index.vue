<template>
  <view class="container">
    <view class="avatar">
      <image class="avatar-img" :src="user.avatarUrl"></image>
      <text class="avatar-name">{{user.nickName}}</text>
    </view>
    <view class="profile">
      <view class="profile-item">
        <text class="profile-label">微信号</text>
        <text class="profile-value">{{user.id}}</text>
      </view>
    <!--  <view class="profile-item">
        <text class="profile-label">姓名</text>
        <text class="profile-value">{{user.name}}</text>
      </view> -->
      <view class="profile-item">
        <text class="profile-label">手机号</text>
        <text class="profile-value">{{user.phone}}</text>
      </view>
      <view class="profile-item">
        <text class="profile-label">地址</text>
        <text class="profile-value">{{user.address}}</text>
      </view>
    </view>
    <button class="edit-btn" type="primary" @click="handleEdit()">编辑</button>
	<button class="edit-btn" type="primary" @click="applyadmin()">申请管理员</button>
  </view>
</template>
<script>
export default {
  data() {
    return {
      user: {
        id: '',
        avatarUrl: 'https://dummyimage.com/200x200/000/fff',
        nickName: '',
        // name: '张三',
        phone: '',
        address: ''
      }
    }
  },
  
  onShow() {
  	 this.getUserInfo()
  },
  methods: {
	   getUserInfo() {
	        // 发送请求获取用户信息，并更新到页面中
			uni.request({
				url: this.$Url + "/Applets/GetUser",
				header: {
					'Content-Type': 'application/json'
				},
				method: 'POST',
				data: {
					"name": this.$member.memberObj.id,
					// "username": this.user.nickName,
					// "address": this.user.address,
					// "phone" : this.user.phoneNumber,
					// "time": new Date().toLocaleTimeString()
				},
				dataType: 'json',
				
				success: (res) => {
					// uni.hideLoading();
					
						// console.log(res.data[0].Username)
						const data = {
							id: this.$member.memberObj.id ,
							avatarUrl: 'https://dummyimage.com/200x200/000/fff',
							nickName: res.data[0].Username,
							// name: '张三',
							phone: res.data[0].Phone,
							address: res.data[0].Address,
						}
						 this.user = data;
						console.log(data.address);
						 // this.user = res.data[0]
								},			
					
				fail: () => {
					uni.hideLoading();
					uni.showToast({
						title: '网络请求失败',
						icon: 'none',
					});
				
				},
				
			})
			
	      },
    handleEdit() {
      uni.navigateTo({
        url: '/pages/edit-profile/edit-profile'
      })
    },
	applyadmin() {
		uni.navigateTo({
		  url: "/pages/applyadmin/applyadmin"
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