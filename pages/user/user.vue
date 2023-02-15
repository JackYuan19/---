<template>
	<view class="user">
		
		<view class="top">
			<image src="../../static/images/history.png" mode="aspectFit"></image>
			<view class="text">浏览历史</view>
		</view>
		
		<view class="content" v-if="historyArr.length !== 0">
			<view class="row" v-for="item in historyArr" :key="item.id">
				<newsbox @click.native="goDetail(item)" :item="item"></newsbox>
			</view>
		</view>
		
		<view class="nohistory" v-else>
			<image src="../../static/images/nohis.png" mode="widthFix"></image>
			<view class="text">
				暂无浏览记录
			</view>
		</view> 
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				historyArr: []
			};
		},
		onShow() {
			this.getData();
		},
		methods: {
			// 跳转到详情页
			goDetail(item) {
				const { classid, id } = item;
				
				uni.navigateTo({
					url: `/pages/detail/detail?cid=${classid}&id=${id}`
				});
			},
			
			// 获取缓存浏览记录
			getData() {
				let historyArr = uni.getStorageSync("historyArr") || [];
				
				this.historyArr = historyArr;
			}
		}
	}
</script>

<style lang="scss">
.user {
	.top {
		padding: 50rpx 0;
		background: #F8F8F8;
		color: #555;
		display: flex;
		justify-content: center;
		align-items: center;
		flex-direction: column;
		image {
			width: 150rpx;
			height: 150rpx;
		}
		.text {
			font-size: 38rpx;
			padding-top: 20rpx;
		}
	}
	
	.content {
		padding: 30rpx;
		.row {
			border-bottom: 1rpx dashed #efefef;
			padding: 20rpx 0;
		}
	}
	
	.nohistory {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		image {
			width: 450rpx;
		}
		.text {
			font-size: 26rpx;
			color: #888;
		}
	}
}
</style>
