]<template>
	<view>
		<uni-search-bar radius="100" bgColor="#ffff"></uni-search-bar>
		<view class="list">
			<view class="row" v-for="(item,index) in orderList" :key='item.id'>
				<view class="products">
					<view class="imagebox" v>
						<image :src="item.child[0].img" mode="widthFix"></image>
						<text>{{item.child[0].goodsname}}</text>
					</view>
				</view>
				<view class="info">
					<text  space="nbsp">总计{{item.countnumber}}商品  </text>  
					<text>应付金额 ￥{{item.countmoney}}</text>
				</view>
			</view>
			
		</view>
		<view class="tishi" v-if="orderList.length==0">
			没有数据亲！
		</view>
	</view>
</template>

<script>
	import uniSearchBar from '@/components/uni-search-bar/uni-search-bar.vue'
	import {
		myRequest,
		baseUrl
	} from '@/utils/db.js'
	export default{
		   components: {uniSearchBar},
		data(){
			return{
				orderList:[],
				
			}
		},
		methods:{
	
		},
		async onShow() {
			 console.log(1)
			let {
				uid,
				token
			} = uni.getStorageSync('userInfo')
			var resultDetail = await myRequest('/api/orders',{uid:uid}, 'GET', {authorization:token})
	resultDetail[1].data.list.forEach(item=>{
			   return  item.child.forEach(m=>{
					 m.img=baseUrl+m.img
				})
		 })
		   this.orderList=resultDetail[1].data.list||[]
		 console.log( this.orderList)
		 }
	}
</script>
<style>

	.row{
		margin: 10rpx 20rpx;
		/* height: 240rpx; */
		border: 1rpx solid #AAAAAA;
		border-radius: 30rpx;
	}
	
	.imagebox{
		float: left;
		margin:10rpx;
	}
	.products{
		overflow: hidden;
	}
	image{
		width: 160rpx;
		margin-left: 10rpx;
	}
	.info{
		/* position: absolute;
		top: 180rpx;
		right: 50rpx; */
		display: flex;
		justify-content: space-between;
		font-size: 25rpx;
		/* width: 340rpx; */
		padding: 20rpx;
		text-align: right;
	}
	.info text:nth-of-type(2){
		color: red;
	}
	.info text:nth-of-type(3){
		font-size: 20rpx;
		color: #AAAAAA;
	}
	.tishi{
		line-height: 200rpx;
		text-align: center;
		font-size: 40rpx;
		color: #006699;
	}
</style>
