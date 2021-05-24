### 1.项目搭建
+ 需要自己跑本地服务
+ 在heima_shop_server命令行执行npm install
+ 在heima_shop_server\src下执行node app.js
+ 在浏览器输入http://localhost:8082/api/getlunbo获取轮播图数据
#### 2.搭建tabbar
+ 在package.json中搭建，具体看文件
#### 3.轮播图
+ 在onLoad函数中使用，页面加载完加载数据
+ //函数会改变this指向，使用箭头函数
+   success:(res)=> {}
##### 3.1对网络请求进行二次封装
+ 拦截请求头
+ api.js

```javascript
const BASE_URL = 'http://localhost:8082'

export const myRequest = (options) => {
	return new Promise((resolve, reject) => {
		uni.request({
			url: BASE_URL + options.url,
			method: options.method,
			data: options.data || {},
			success: (res) => {
				if (res.data.status !== 0) {
					return uni.showToast({
						title: "获取数据失败"
					})
				}
				resolve(res)
			},
			fail: (err) => {
                uni.showToast({
					title: "请求接口失败"
				})
				reject(err)
			}
		})
	})
}

```

+ **在main.js中**

```javascript
import { myRequest } from './util/api.js'
//定义到原型上，挂载到全局
Vue.prototype.$myRuquest = myRequest
```

+ **在首页使用时**

```
onLoad() {
        this.getSwipers()
	},
	methods: {
	  // 获取轮播图的数据
	  getSwipers () {
		  console.log("获取轮播图的数据")
		 //  uni.request({
		 //  	url:'http://localhost:8082/api/getlunbo',
			// //函数会改变this指向，使用箭头函数
			// success:(res)=> {
			// 	console.log(res)
			// 	if(res.data.status !== 0){
			// 		uni.showToast({
			// 			title: "获取数据失败"
			// 		})
			// 	}
			// 	this.swipers = res.data.message
			// }
		 //  })
		 //直接使用this.$myRuquest
		 const res = this.$myRuquest({
			 url: '/api/getlunbo'
		 })
		 console.log(res)
	  }
	}
```

### 3.商品列表 -->图片地址失效

+ padding: 15rpx;
+ box-sizing: border-box;
+ 当使用margin或者padding把盒子撑大的时候，使用box-sizing：border-box
+ 盒子不会撑大，内容被挤小
+ text-decoration: line-through;中划线
+ display: block;  margin: 0 auto;，块元素居中

### 开启页面刷新

+ 在pages.json中的页面设置中开启
+ 在style中设置"enablePullDownRefresh": true



页面生命函数中**onPullDownRefresh**监听下拉刷新

数据请求完后阻止下拉刷新，将阻止下拉刷新的函数传入请求数据的函数中

```
setTimeout(()=>{
    this.getGoodsList(()=>{
        uni.stopPullDownRefresh()
    })
},1000)
```

在请求数据的函数中，使用callback接收函数

callBack&&callBack()，如果有就刷新，防止第一次就刷新



富文本，在uni中使用

<rich-text :nodes="content"> 
				</rich-text>