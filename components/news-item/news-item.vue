<template>
	<view>
		<view class="news-item" v-for="item in list" :key="item.id" @click="navigator(item.id)">
			<image src="item.img_url"></image>
			<view class="right">
				<view class="tit">
					{{item.title}}
				</view>
				<view class="info">
					<text>发表时间：{{item.add_time | formatDate}}</text>
					<text>浏览次数：{{item.click}}</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default{
		props: ['list'],
		// 这里是可以删掉的，但是这是我们封装的组件，在哪里都能用，所以不建议删除
		filters: {
			formatDate (date){
				const nDate = new Date(date)
				const year = nDate.getFullYear().toString().padStart(2,0)
				const month = nDate.getMonth().toString().padStart(2,0)
				const day = nDate.getDay()
				return year+'-'+month+'-'+day
			}
		},
		methods:{
			navigator(id){
				this.$emit('itemClick', id)
			}
		}
	}
</script>

<style lang="less">
	.news-item{
		display: flex; 
		padding: 10rpx 20rpx;
		border-bottom: 1px solid red;
		image{ 
			max-width: 200rpx;
			min-width: 200rpx;
			height: 150rpx;
			max-height: 150rpx;
		}
		.right{
			margin-left: 15rpx;
			display: flex;
			flex-direction: column;
			justify-content: space-between;
			.tit{
				font-size: 30rpx;
			}
			.info{
				font-size: 24rpx;
				text:nth-child(2){
					margin-left: 30rpx;
				}
			}
		}
	}
</style>
