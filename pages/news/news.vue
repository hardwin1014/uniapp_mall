<template>
	<view class="news">
		<news-item :list="newsList" @itemClick="goDetail($event)"></news-item>
	</view>
</template>

<script>
	import newsItem from '../../components/news-item/news-item.vue'
	export default {
		data() {
			return {
				newsList: []
			}
		},
		methods: {
			async getNews () {
				const res = await this.$myRuquest({
					url: '/api/getnewslist'
				})
				// console.log(res)
				this.newsList = res.data.message
			},
			goDetail (id){
				console.log(id)
				uni.navigateTo({
					url: `/pages/news-detail/news-detail?id=${id}`
				})
			}
		},
		onLoad(){
			this.getNews()
		},
		components:{
			"news-item":newsItem
		}
	}
</script>

<style lang="less">
.news{
	
}
</style>
