<template>
	<view>
		<showtime class="timeclass"></showtime>
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
					<view class="data-cell" @click="handleClick()">预约</view>
				</view>
			</scroll-view>
			<uni-drawer ref="showRight" mode="right" :mask-click="false">
				<button @click="closeDrawer" type="primary">申请预约</button>
				<Mypicker></Mypicker>
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



				// 表格内容部分的高度
				contentHeight: 0
			};
		},

		onLoad() {
			uni.request({
				url: "http://8itjmc.natappfree.cc/Applets/GetRoom",
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

					// console.log(res.data.data)
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
				this.$refs.showRight.open();
			},
			closeDrawer() {
				this.$refs.showRight.close();
			},
			// 点击事件处理函数
			handleClick() {
				// 从表格数据中获取被点击的单元格的值
				// const cellValue = this.data[rowIndex][cellIndex];
				// console.log(cellValue);

				// 如果点击的是第四列，则显示弹窗
				// if (cellIndex === 3) {
				// 	const cellValue = this.data[rowIndex][cellIndex];
				// 	console.log(cellValue);
					this.showDrawer()
					
			},
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
