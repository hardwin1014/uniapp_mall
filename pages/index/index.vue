<template>
	<view class="home">
		<swiper indicator-dots circular>
			<swiper-item v-for="item in swipers" :key="item.id">
				<image :src="item.img"></image>
			</swiper-item>
		</swiper>
		<!-- 导航区域 -->
		<view class="nav">
			<view class="nav_item" 
			      v-for="(item, index) in navs" 
				  :key="index"
				  @click="navItemClick(item.path)">
				<view :class="item.icona"></view>
				<text>{{item.title}}</text>
			</view>
		</view>
		<!-- 推荐商品 -->
		<view class="hot_goods">
			<view class="tit">推荐商品</view>
			<goods-list :goods="goods" @goodsItemClick="goGoodsDetail"></goods-list>
		</view>
	</view>
</template>

<script>
	import goodsList from '../../components/good-list/goods-list.vue'
	export default {
		data() {
			return {
				swipers: [], //轮播数据
				goods: [], //商品列表
				navs: [{
						icona: 'iconfont icon-ziyuan',
						title: '黑马超市',
						path: '/pages/goods/goods'
					},
					{
						icona: 'iconfont icon-guanyuwomen',
						title: '联系我们',
						path: '/pages/contact/contact'
					},
					{
						icona: 'iconfont icon-tupian',
						title: '社区图片',
						path: '/pages/pics/pics'
					},
					{
						icona: 'iconfont icon-shipin',
						title: '学习视频',
						path: '/pages/videos/videos'
					}
				]
			}
		},
		onLoad() {
			this.getSwipers()
			this.getHotGoods()
		},
		components:{
			"goods-list": goodsList
		},
		methods: {
			// 获取轮播图的数据
			async getSwipers() {
				const res = await this.$myRuquest({
					url: '/api/getlunbo'
				})
				// console.log(res)
				this.swipers = res.data.message
			},
			// 获取热门商品列表数据
			async getHotGoods() {
				const res = await this.$myRuquest({
					url: '/api/getgoods?pageindex=1'
				})
				// console.log(res)
				this.goods = res.data.message
			},
			// 
			navItemClick(url){
				// console.log('跳转')
				uni.navigateTo({
					url
				})
			},			
			//导航到商品详情页面
			goGoodsDetail (id) {
				uni.navigateTo({
					url: '/pages/goods-detail/goods-detail?id='+id
				})
			}
		}
	}
</script>

<style lang="scss">
	.home {
		swiper {
			width: 750rpx;
			height: 380rpx;

			image {
				width: 100%;
				height: 100%;
			}
		}

		.nav {
			width: 750rpx;
			display: flex;

			.nav_item {
				width: 25%;
				text-align: center;

				view {
					width: 120rpx;
					height: 120rpx;
					background: $shop-color;
					border-radius: 60rpx;
					margin: 10px auto;
					line-height: 120rpx;
					color: #fff;
					font-size: 50rpx;
				}

				.icon-tupian {
					font-size: 45rpx;
				}

				text {
					font-size: 30rpx;
				}
			}
		}

		.hot_goods {
			background-color: #eee;
			overflow: hidden;
			margin-top: 10px;

			.tit {
				height: 50px;
				line-height: 50px;
				color: $shop-color;
				text-align: center;
				letter-spacing: 20px;
				background: #fff;
				margin: 7rpx 0;
			}
		}
	}
</style>
