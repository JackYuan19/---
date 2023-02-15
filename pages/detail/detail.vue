<template>
	<view class="detail">
		
		<view class="title">
			{{detail.title}}
		</view>
		
		<view class="info">
			<view class="author">
				编辑：{{detail.author}}
			</view>
			
			<view class="time">
				{{detail.posttime}}
			</view>
		</view>
		
		<view class="content">
			<rich-text :nodes="detail.content"></rich-text>
		</view>
		
		<view class="description">
			声明:本站的内容均采集与腾讯新闻,如果侵权请联系管理（513894357@qq.com）
			进行整改删除,本站进行了内容采集不代表本站及作者观点,若有侵犯请及时联系管理员，
			谢谢您的支持。
		</view>
		
	</view>
</template>

<script>
	import { parseTime } from "../../utils/tool.js";
	
	export default {
		data() {
			return {
				options: null,
				detail: {
					
				}
			};
		},
		onLoad(e) {
			this.options = e;
			this.getDetail();
		},
		methods: {
			// 获取新闻详情列表
			getDetail() {
				const { options } = this;
				if (options === null) {
					return;
				}
				
				uni.request({
					url: "https://ku.qingnian8.com/dataApi/news/detail.php",
					data: options,
					success: (res) => {
						res.data.content = res.data.content.replace(/<img/gi,'<img style="max-width: 100%"');
						res.data.posttime = parseTime(res.data.posttime);
						this.detail = res.data;
						
						this.saveHistory();
						
						uni.setNavigationBarTitle({
							title: res.data.title
						})
					}
				});
			},
			
			// 保存历史记录
			saveHistory() {
				const { id, classid, picurl, title } = this.detail;
				
				let historyArr = uni.getStorageSync("historyArr") || [];
				
				let index = historyArr.findIndex(item => {
					return item.id === id;
				});
				if (index >= 0) {
					historyArr.splice(index, 1);
				}
				
				historyArr.unshift({
					id: id,
					classid: classid,
					picurl: picurl,
					title: title,
					looktime: parseTime(Date.now())
				});
				historyArr = historyArr.slice(0, 10);
				
				uni.setStorageSync("historyArr", historyArr);
			}
		}
	}
</script>

<style lang="scss">
.detail {
	padding: 30rpx;
	.title {
		font-size: 46rpx;
		color: #333;
	}
	.info {
		background: #F6F6F6;
		padding: 20rpx;
		font-size: 25rpx;
		color: #999;
		display: flex;
		justify-content: space-between;
		margin: 30rpx 0;
	}
	
	.content {
		padding-bottom: 50rpx;
	}
	
	.description {
		background: #FEF0F0;
		font-size: 26rpx;
		padding: 20rpx;
		color: #F89898;
		line-height: 1.8em;
	}
}
</style>
