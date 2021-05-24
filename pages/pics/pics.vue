<template>
	<view class="pics">
		<scroll-view class="left" scroll-y>
			<view :class="active===index?'active':''"
			      v-for="(item, index) in cates" 
				  :key='item.id'
				  @click="leftClickHandle(index, item.id)"
		    >{{item.title}}</view>
		</scroll-view>
		<!-- scroll-y允许纵向滑动 -->
		<scroll-view class="right" scroll-y>
			<view class="item" v-for="item in secondData" :key="item.id">
				<image @click="previewImg(item.img_url)" :src="item.img_url"></image>
			    <text>{{item.title}}</text>
			</view>
			<!-- 当数据为0的时候显示暂无数据 -->
			<text v-if="secondData.length===0">暂无数据</text>
		</scroll-view>
	</view>
</template>

<script>
export default {
	data() {
		return {
            cates: [],
			active: 0,
			secondData: []
		}
	},
	methods: {
		// 获取图片数据
        async getPicsCate () {
			const res = await this.$myRuquest({
				url: '/api/getimgcategory'
			})
			console.log(res)
			this.cates = res.data.message
			// 获取数据之后，然后默认显示第一个,没数据也得获取一次
			this.leftClickHandle(0,this.cates[0].id)
		},
		async leftClickHandle(index,id){
		    this.active = index
			// 获取右侧的数据..需要id传一个item.id
			const res = await this.$myRuquest({
				url: '/api/getimages/'+id
			})
			// console.log(res)
			this.secondData = res.data.message
		},
		previewImg(current){
			// 先从数据中获取图片地址
			const urls = this.secondData.map(item=>{
			    return item.img_url
			})
			// console.log(urls)
			// 然后再给预览图片的组件
			uni.previewImage({
				current,
				urls
			})
		}
	},
	onLoad(){
		this.getPicsCate()
	}
}
</script>

<style lang="scss">
page {
	height: 100%
}
.pics {
	height: 100%;
	display: flex;
	.left {
		width: 200rpx;
		height: 100%;
		border-right: 1px solid #eee;
		view {
			height: 60px;
			line-height: 60px;
			color: #333;
			text-align: center;
			font-size: 30rpx;
			border-top: 1px solid #eee;
		}
		.active{
			background: $shop-color;
			color: #fff;
		}
	}
	.right{
		height: 100%;
		width: 520rpx;
		margin: 10rpx auto;
		.item{
			image{
				width:520rpx;
			    height: 520rpx;
			    border-radius: 5px;
			}
			text{
				font-size: 30rpx;
				line-height: 60rpx;
			}
		}
	}
}
</style>
