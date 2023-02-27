<template>
	<view>
		<button @click="showDrawer" type="primary">右侧弹出 显示Drawer</button>
		<uni-drawer ref="showRight" mode="right" :mask-click="false">
			<button @click="closeDrawer" type="primary">关闭Drawer</button>	
			<Mypicker></Mypicker>
		</uni-drawer>
	</view>
</template>

<script>
	import Mypicker  from "@/components/Mypicker/Mypicker.vue"
	export default {
		components:{
			Mypicker
		},
		name:"myuni-drawer",
		data() {
			return {
				
			};
		},
			methods: {
					showDrawer() {
						this.$refs.showRight.open();
					},
					closeDrawer() {
						this.$refs.showRight.close();
					}
		
				}
			}
	
</script>

<style lang="less">

</style>