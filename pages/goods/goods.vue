<template> 
	<view class="goods_list">
		<good-list :goods="goods" @goodsItemClick="goGoodsDetail"></good-list>
		<view class="isOver" v-if="flag">-----我也是有底线的-----</view>
	</view> 
</template>

<script>
import goodsList from '../../components/good-list/goods-list.vue'
export default {
	data() {
		return {
			pageindex: 1,
			goods: [],
			flag: false
		}
	}, 
	components:{
		"good-list":goodsList
	}, 
	methods: {
		//获取商品列表数据
		async getGoodsList(callBack) {
			const res = await this.$myRuquest({
				url: '/api/getgoods?pageindex='+this.pageindex
			})
			// console.log(res)
			//将数组展开，合并
			this.goods = [...this.goods,...res.data.message]
			// this.goods.push(...res.data.message)
			
			//页面刚开始请求数据的时候
			//判断是否有callback函数，如果有就刷新
			callBack&&callBack()
		},
		//导航到商品详情页面
		goGoodsDetail (id) {
			uni.navigateTo({
				url: '/pages/goods-detail/goods-detail?id='+id
			})
		}
	},
	// 
	onLoad () {
		this.getGoodsList()
	},
	//页面触底触发
	onReachBottom() {
		//如果商品个数小于页面数乘以10
		if(this.goods.length < this.pageindex*10){
			return this.flag = true
		}
		//页面触底，将页面值+1，然后再执行 getGoodsList() 
		this.pageindex++;
		this.getGoodsList() 
	},
	// 监听下拉刷新
	onPullDownRefresh() {
		console.log("下拉刷新了")
		this.pageindex = 1
        this.goods = [],
		this.flag = false,
		//重新获取数据  会闪一下所以使用延迟
		setTimeout(()=>{
			this.getGoodsList(()=>{
				uni.stopPullDownRefresh()
			})
		},1000)
	}
}
</script>

<style lang="scss">
.goods_list{
	background-color: #eee;
}
.isOver{
	width: 100%;
	height: 50%;
	line-height: 50px;
	text-align: center;
	font-size: 28rpx; 
}
</style>
