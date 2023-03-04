<template>
	<view>
		<showtime class="timeclass" ref="choosedate"></showtime>
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
					<!-- @click="handleClick(row, cell, rowIndex, cellIndex)" -->
					<!-- <button type="primary" size="mini" @click="handleClick()">预约</button> -->
					<view class="data-cell" v-show="true">
						<button type="primary" size="mini" v-model="indexvalue"
							@click="handleClick(row, cell, rowIndex, cellIndex)" >预约</button>
					</view>
					
					<view class="data-cell" v-show="false">
						<button type="primary" size="mini" v-model="indexvalue"
							@click="handleClick(row, cell, rowIndex, cellIndex)" disabled>预约</button>
					</view>
				</view>
			</scroll-view>
			<uni-drawer ref="showRight" mode="right" :mask-click="false">
				<!-- <Mypicker ref = "pick"></Mypicker> -->
				<form>
					<view class="form-item">
						<view class="label">联系人：</view>
						<input type="text" placeholder="请输入联系人姓名" v-model="contactName" />
					</view>

					<view class="form-item">
						<view class="label">预约时间</view>
						<Mypicker ref="pick"></Mypicker>
					</view>

					<view class="form-item">
						<view class="label">联系电话：</view>
						<input type="tel" placeholder="请输入联系人电话" v-model="contactPhone" />
					</view>

					<view class="form-item">
						<view class="label">预约人数：</view>
						<input type="number" placeholder="请输入预约人数" v-model="num" />
					</view>

					<uni-section title="申请理由" subTitle="申请理由" type="line" padding>
						<uni-easyinput type="textarea" autoHeight v-model="value" placeholder="请输入内容" clearable="true">
						</uni-easyinput>
						<!-- <button @click="submit" type="primary" size="mini">提交</button> -->
					</uni-section>
				</form>
				<button @click="submitForm" type="primary">提交预约</button>
				<button @click="closeDrawer" type="primary">取消预约</button>
				<!-- <Mypicker ref = "pick"></Mypicker> -->
			</uni-drawer>
		</view>
	</view>
</template>

<script>
	import showtime from '@/components/showtime/showtime.vue'
	import Mypicker from '@/components/Mypicker/Mypicker.vue'
	export default {

		components: {
			showtime,
			Mypicker
		},
		data() {
			return {
				// 表头数据
				header: [{
						"name": "会议室"
					},
					{
						"name": "会议室大小"
					},
					{
						"name": "备注"
					},
					{
						"name": "预约"
					}
				],
				// 表格数据
				//     data: [
				//       ["逸夫楼108", 80, "地址", "预约"],
				//       ["逸夫楼205", 90, "地址", "预约"],
				//       ["图书馆103", 120, "地址", "预约"],
				// ["五教101", 45, "地址", "预约"],
				// ["三教312", 60, "地址", "预约"],
				// ["四教111", 30, "地址", "预约"],
				// ["十教208", 55, "地址", "预约"],
				//       ["八教409", 80, "地址", "预约"]
				//     ],
				data: [],
				contactPhone: '',
				contactName: '',
				value: '',
				num: '',
				indexvalue: '',



				// 表格内容部分的高度
				contentHeight: 0
			};
		},

		onLoad() {
			uni.request({
				url: this.$Url + "/Applets/GetRoom",
				header: {
					'Content-Type': 'application/json'
				},
				method: 'POST',
				data: {

				},
				dataType: 'json',
				success: (res) => {
					// const data = JSON.parse(res.data)
					// var obj = new Object();
					// obj = res

					console.log(res.data[0])
					this.data = res.data;


				}

			})

		},
		mounted() {
			// 计算表格内容部分的高度
			this.getContentHeight();
		},
		methods: {
			// 计算表格内容部分的高度
			getContentHeight() {
				uni.createSelectorQuery()
					.in(this)
					.select('.table-content')
					.boundingClientRect((rect) => {
						this.contentHeight = rect.height;
					})
					.exec();
			},

			showDrawer() {

				// console.log(this.$refs.choosedate. formateDate(this.$refs.choosedate.date));
				// this.$refs.choosedate. formateDate(this.$refs.choosedate.value)
				this.$refs.showRight.open();
			},
			closeDrawer() {

				// this.$refs.pick.submitForm();
				this.clearForm();
				this.$refs.showRight.close();
			},

			submitForm() {
				// if (!this.name || !this.phone || !this.date)

				if (!this.contactName || !this.contactPhone) {
					uni.showToast({
						title: '请填写完整信息',
						icon: 'none',
					});
					console.log(this.contactPhone);
					return;
				}
				// if (!/^1[3456789]\d{9}$/.test(this.phone)) {
				//   uni.showToast({
				//     title: '手机号码格式不正确',
				//     icon: 'none',
				//   });
				//   return 1;
				// }

				uni.showLoading({
					title: '提交中...',
				});
				uni.request({
						url: this.$Url + "/Applets/ApplyReserve",
						method: 'POST',
						data: {
							// roomName: this.data[rowIndex][0],
							// date: this.date,
							room_name: this.indexvalue,
							name: this.contactName,
							phone: this.contactPhone,
							num: this.num,
							reason: this.value,
							date: this.$refs.choosedate.formateDate(this.$refs.choosedate.date),
							period: this.$refs.pick.selected,
						},


						success: (res) => {
							uni.hideLoading();
							console.log(res.data);
							if (res.data.msg === "ok") {
								uni.showToast({
									title: '预约成功',
									icon: 'success',
								});

								this.clearForm();
								this.$refs.showRight.close();

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

					}),
					this.clearForm();
			},
			clearForm() {
				this.contactPhone = '';
				this.num = '';
				this.value = '';
				this.contactName = '';
				this.$refs.pick.selected = '请选择预约时间段';

				// this.remark = '';
			},

			// 点击事件处理函数
			handleClick(row, cell, rowIndex, cellIndex) {
				// 从表格数据中获取被点击的单元格的值
				// const cellValue = row[cellIndex];
				console.log(row.Xx);
				this.indexvalue = row.Xx;
				console.log(this.indexvalue);

				// 如果点击的是第四列，则显示弹窗
				// if (cellIndex === 3) {
				// 	const cellValue = this.data[rowIndex][cellIndex];
				// 	console.log(cellValue);
				// this.indexvalue = this.data[rowIndex][0];
				// console.log(cellValue);
				// const cellValue = this.data[0].xx;
				// console.log(cellValue);
				this.showDrawer();

			},
			buttonshow() {
				uni.request({
					url: this.$Url + "/Applets/ApplyReserve",
					method: 'POST',
					data: {
						// roomName: this.data[rowIndex][0],
						// date: this.date,
						room_name: this.indexvalue,
						// name: this.contactName,
						// phone: this.contactPhone,
						// num: this.num,
						// reason: this.value,
						date: this.$refs.choosedate.formateDate(this.$refs.choosedate.date),
						// period: this.$refs.pick.selected,
					},
					
					success: (res) => {
						uni.hideLoading();
						console.log(res.data);
						if (res.data.msg === "ok") {
							 
						} else {
							
					}
					
					}
					
				})
			}
		}
	};
</script>

<style scoped>
	.table-container {
		display: flex;
		flex-direction: column;
		width: 100%;
		height: 100%;
	}

	.timeclass {
		/* display: flex;
	position: fixed;
	position: sticky;
	top: 0;
	left: 0;
	right: 0;
	background-color: #fff;
	z-index: 1; */
		display: flex;
		position: fixed;
		position: sticky;
		top: 0;
		left: 0;
		right: 0;
		background-color: #fff
	}

	.header-row {
		display: flex;
		position: fixed;
		position: sticky;
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
		margin-top: 0px;
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
