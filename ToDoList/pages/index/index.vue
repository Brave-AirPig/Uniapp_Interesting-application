<template>
	<view class="todolist">
		
		<form class="form" @submit="formSubmit">
			<view class="ipt">
				<input type="text" v-model="value" placeholder="制定您的绝密计划" name="text">
				<button form-type="submit" class="btn"> + </button>
			</view>
		</form>
		
		<view class="tips_text" v-if="num">
			<text>还有 <text class="number"> {{ num }} </text> 件未完成事件<text class="number mark"> 🚀 </text></text>
		</view>
		
		<view class="lists">
			<view class="list" v-for="item in list" :key="item.id">
				<view class="success">
					<image src="../../static/images/ok_yes.png" mode="heightFix"></image>
				</view>
				<view class="contant" @click="navTo(item.id)">
					{{ item.text }}
				</view>
				<view class="fail" @click="remove(item.id)">
					<image src="../../static/images/fail.png" mode="heightFix"></image>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list: [],
				value:'',
				num:null
			}
		},
		async onLoad() {
			this.list = Array.from(JSON.parse(JSON.stringify(uni.getStorageSync('todoList'))))
			this.num = this.list.length
		},
		methods: {
			formSubmit(e) {
				if(this.value.trim()) {
					this.value = ''
					this.list.unshift({id:this.list.length,text:e.detail.value.text})
					this.num = this.list.length
					uni.setStorageSync('todoList',this.list)
					console.log(this.list)
				} else {
					this.value = ''
				}
			},
			remove(id) {
				for(let i = 0;i < this.list.length;i++){
					if(this.list[i].id === id) {
						this.list.splice(i, 1)
						this.num = this.list.length
						uni.setStorageSync('todoList',this.list)
					}
				}
			},
			navTo(id) {
				uni.navigateTo({
					url:`/pages/message/message?id=${id}`
				})
			}
		}
	}
</script>

<style lang="less">
	.ipt {
		padding:24rpx;
		background-color: #404040;
		display: flex;
		grid-template-columns: 8fr 2fr;
		input {
			flex: 9;
			background-color: #efefef;
			padding: 24rpx;
			border-radius: 24rpx;
			margin-right: 12rpx;
		}
		.btn {
			flex: 1;
			background-color: #efefef;
			font-size: 64rpx;
			font-weight: bold;
			line-height: 86rpx;
		}
	}
	
	.list {
		display: grid;
		grid-template-columns: 2fr 7fr 1fr;
		margin: 32rpx 24rpx;
		background-color: #404040;
		color: #efefef;
		padding: 48rpx;
		border-radius: 24rpx;
		font-size: 32rpx;
		box-shadow: 2rpx 2px 4px #404040;
		.contant {
			padding: 0 32rpx;
			white-space: nowrap;
			overflow: hidden;
			text-overflow: ellipsis;
		}
		.success,
		.fail {
			width: 100%;
			height: 100%;
			font-weight: bold;
			image {
				width: 100%;
				height: 100%;
			}
		}
	}
	
	.tips_text {
		text-align: center;
		margin: 24rpx 0;
		.number {
			font-weight: bold;
			font-size: 48rpx;
			color: #aa0000;
		}
	}
</style>
