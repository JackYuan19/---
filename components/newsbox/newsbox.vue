<template>
	<view class="newsbox">
		
		<view class="pic">
			<image :src="item.picurl" mode="aspectFit"></image>
		</view>
		
		<view class="text">
			<view class="title">{{item.title}}</view>
			<view class="info" v-if="!item.looktime">
				<text>{{item.author}}</text>
				<text>{{item.hits}}浏览</text>
			</view>
			<view class="info" v-else>
				<text>浏览时间：{{item.looktime}}</text>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		name:"newsbox",
		props: {
			item: {
				type: Object,
				default() {
					return {
						title: "组件内默认的标题",
						author: "张三",
						hits: 668,
						picurl: "../../static/images/nopic.jpg",
						looktime: "2022-12-12 10:10:20"
					}
				}
			}
		},
		data() {
			return {
				
			};
		}
	}
</script>

<style lang="scss">
.newsbox {
	display: flex;
	.pic {
		width: 230rpx;
		height: 160rpx;
		image {
			width: 100%;
			height: 100%;
		}
	}
	.text {
		flex: 1;
		padding-left: 20rpx;
		.title {
			font-size: 36rpx;
			color: #333;
			overflow : hidden;
			text-overflow: ellipsis;
			display: -webkit-box;
			-webkit-line-clamp: 2;
			-webkit-box-orient: vertical;
		}
		.info {
			font-size: 26rpx;
			color: #999;
			text {
				padding-right: 30rpx;
			}
		}
	}
}
</style>