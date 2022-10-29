<template>
	<view>
		<view class="message">
			<text>{{ message }}</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				message:''
			}
		},
		onLoad(option) {
			const list = uni.getStorageSync('todoList')
			for(let i = 0;i < list.length;i++) {
				if(list[i].id === Number(option.id)) {
					this.message = list[i].text
				}
			}
		}
	}
</script>

<style lang="less">
	page {
		background-color: #efefef;
	}
	.message {
		margin: 24rpx;
		font-weight: bold;
		font-size: 32rpx;
		color: #404040;
		line-height: 1.8;
		border: 1px solid #404040;
		padding: 24rpx;
		border-radius: 24rpx;
	}
</style>
