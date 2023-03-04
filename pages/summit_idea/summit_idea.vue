<template>
	<view class="container">
		<view class="header">
			<text>历史预约记录</text>
		</view>
		<view class="list">
			<view v-for="(item, index) in historyList" :key="index" class="item">
				<view class="top">
					<text class="title">{{ item.RoomName }}</text>
					<text class="status">{{ item.Status }}</text>
				</view>
				<view class="bottom">
					<text class="date">{{ item.Date }}</text>
					<text class="time">{{ item.Period }}</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				historyList: [
							{room: '',
							status: '',
							date: '',
							time: ''}
				],
				// {
				// 		room: 'A101',
				// 		status: '已取消',
				// 		date: '2022-02-10',
				// 		time: '09:00-10:00'
				// 	},
				// 	{
				// 		room: 'A102',
				// 		status: '已使用',
				// 		date: '2022-02-11',
				// 		time: '14:00-16:00'
				// 	},
				// 	{
				// 		room: 'A103',
				// 		status: '已完成',
				// 		date: '2022-02-12',
				// 		time: '15:00-16:00'
				// 	}
			}
		},
		onLoad() {
			this.gethistoryList()
		},

		methods: {
			gethistoryList() {

				uni.request({

					url: this.$Url + "/Applets/GetRecord",
					header: {
						'Content-Type': 'application/json'
					},
					method: 'POST',
					data: {
						"name": this.$member.memberObj.id,
					},
					dataType: 'json',
					
					success: (res) => {
					// 	uni.hideLoading();
					// 	if (res.data.msg === "ok") {
					// 		uni.hideLoading()
					// 		uni.showToast({
					// 			title: '申请发送成功',
					// 			icon: 'success'
					// 		})
					
					// 	}
					// this.historyList = res.data;
					// const data = [
					// 	{
					// 		room: '',
					// 		status: '',
					// 		date: '',
					// 		time: ''
					// 	}
					// ];
					
					// for(var i = 0;i < res.data.length; i++ ) {
						
					// 	 this.historyList[i].date = res.data[i].Date;
					// 	this.historyList[i].time = res.data[i].Period;
					// 	 this.historyList[i].room = res.data[i].RoomName;
					// 	 if(res.data[i].Status == 1)
					// 	 {
					// 		 this.historyList[i].date.status ="审核中";
					// 	 } else if(res.data[i].Status == 0) {
					// 		  this.historyList[i].date.status ="未通过";
					// 	 } else {
							 
					// 		 this.historyList[i].date.status ="通过";
					// 	 }

					// }
					 this.historyList = res.data;
					// data.date =res
					console.log(this.historyList);
					}
					
					
				})
				

			}
		}
	}
</script>

<style>
	.container {
		padding: 20px;
	}

	.header {
		margin-bottom: 20px;
	}

	.list {
		background-color: #fff;
		padding: 20px;
		border-radius: 10px;
		box-shadow: 0px 0px 10px #888;
	}

	.item {
		margin-bottom: 20px;
		border-bottom: 1px solid #ccc;
	}

	.top {
		display: flex;
		justify-content: space-between;
		align-items: center;
	}

	.title {
		font-size: 16px;
		font-weight: bold;
	}

	.status {
		font-size: 14px;
		color: #f00;
	}

	.bottom {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin-top: 10px;
	}

	.date {
		font-size: 14px;
	}

	.time {
		font-size: 14px;
		color: #666;
	}
</style>
