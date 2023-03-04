<template>
	<view>
		<picker mode="selector" :range="options" @change="onSelect" v-model="selected">
			<view class="picker">
				{{ selected }}
			</view>
		</picker>

		<!-- <button class="btn-submit" type="primary" @click="submitForm">提交预约</button> -->
	</view>
</template>

<script>
	export default {
		name: "Mypicker",
		data() {
			return {
				options: ['8:00-10:00', '10:15-12:15', '14:00-16:00', '16:15-18:15', '20:00-22:00'],
				selected: '请选择预约时间段',
				contactPhone: '',
				contactName: '',
				// name: '',
				// phone: '',
			}
		},
		methods: {
			onSelect(event) {
				const {
					value
				} = event.detail
				this.selected = this.options[value]
			}

		},

	}
</script>

<style>
	.form-item {
		display: flex;
		align-items: center;
		margin-bottom: 20rpx;
	}

	.label {
		width: 160rpx;
		font-size: 30rpx;
		color: #333;
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
		bottom: 5rpx;
	}
</style>
