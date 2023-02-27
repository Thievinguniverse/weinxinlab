<template>
  <view class="container">
    <form>
      <view class="form-item">
        <view class="label">预约时间：</view>
        <view class="value">{{date}}</view>
      </view>
      <view class="form-item">
        <view class="label">预约用途：</view>
        <input type="text" placeholder="请输入预约用途" v-model="purpose" />
      </view>
      <view class="form-item">
        <view class="label">预约人数：</view>
        <input type="number" placeholder="请输入预约人数" v-model="num" />
      </view>
      <view class="form-item">
        <view class="label">联系人：</view>
        <input type="text" placeholder="请输入联系人姓名" v-model="contactName" />
      </view>
      <view class="form-item">
        <view class="label">联系电话：</view>
        <input type="tel" placeholder="请输入联系人电话" v-model="contactPhone" />
      </view>
      <button class="btn-submit" type="primary" @click="submitForm">提交预约</button>
    </form>
  </view>
</template>


<script>
  export default {
    data() {
      return {
        roomName: '会议室A',
        now: new Date().getTime(),
        endDate: new Date(new Date().getFullYear() + 1, 11, 31).getTime(),
        date: '',
        name: '',
        phone: '',
        remark: '',
      };
    },
    methods: {
      onDateChange(e) {
        this.date = e.detail.value;
      },
      submitForm() {
        if (!this.name || !this.phone || !this.date) {
          uni.showToast({
            title: '请填写完整信息',
            icon: 'none',
          });
          return;
        }
        if (!/^1[3456789]\d{9}$/.test(this.phone)) {
          uni.showToast({
            title: '手机号码格式不正确',
            icon: 'none',
          });
          return;
        }
        uni.showLoading({
          title: '提交中...',
        });
        uni.request({
          url: 'https://example.com/api/booking',
          method: 'POST',
          data: {
            roomName: this.roomName,
            date: this.date,
            name: this.name,
            phone: this.phone,
            remark: this.remark,
          },
          success: (res) => {
            uni.hideLoading();
            if (res.data.code === 0) {
              uni.showToast({
                title: '预约成功',
                icon: 'success',
              });
              this.clearForm();
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
        });
      },
      clearForm() {
        this.date = '';
        this.name = '';
        this.phone = '';
        this.remark = '';
      },
    },
  };
</script>

<style>
  .container {
    padding: 20rpx;
  }
  .form-item {
    display: flex;
    align-items: center;
    margin-bottom: 20rpx;
  }
  .label {
    width: 160rpx;
    font-size: 28rpx;
    color: #333;
  }
  .value {
    flex: 1;
    font-size: 28rpx;
    color: #666;
  }
  input,
  textarea {
    flex: 1;
    height: 100rpx;
    padding: 10rpx;
    border: 1rpx solid #ccc;
    border-radius: 10rpx;
    font-size: 28rpx;
    color: #666;
  }
  .btn-submit {
    display: block;
    width: 100%;
    height: 100rpx;
    line-height: 100rpx;
    text-align: center;
    background-color: #007aff;
    color: #fff;
    font-size: 32rpx;
    border-radius: 10rpx;
  }
  
  .value {
    flex: 1;
    font-size: 28rpx;
    color: #666;
    white-space: pre-wrap; /* 新增的样式 */
  }

</style>

