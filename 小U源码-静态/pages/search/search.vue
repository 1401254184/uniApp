<template>
	<view>
		<uni-search-bar :radius="100" bgColor="#ffffff" class="input_"></uni-search-bar>
		<view class="list">
			<view class="row" v-for="(item,index) in caselist" :key='item.id'>
				<image :src="item.img" mode="widthFix"></image>
				<view class="info">
					<text>{{item.goodsname}}</text>	
					<text>原价￥{{item.market_price}}</text>	
					<text>现价￥{{item.price}}</text>	
					<text>3565评论</text>	
				</view>
			</view>
			
		</view>
		<view class="tishi" v-if="caselist.length==0">
			没有数据亲！
		</view>
	</view>
</template>

<script>
import{myRequest,baseUrl}from'@/utils/db.js'
	export default{
		 data(){
			 return{
				 caselist:[]
			 }
		 },
		 methods:{ 
		 },
	async onLoad(e){
		 var keywords=e.keywords
		 console.log(keywords)
		 var result=await myRequest('/api/search',{'keywords':keywords})
		console.log(result)
		
		 if(result[1].data.list!=null){
			  this.caselist=result[1].data.list
		 this.caselist.forEach(item=>{
			 item.img=baseUrl+item.img
		 })
		}}
		
	}
</script>

<style>

	.row{
		position: relative;
		margin: 10rpx 20rpx;
		height: 190rpx;
		border-bottom: 1rpx solid #AAAAAA;
	}
	image{
		position: absolute;
		top: 0;
		left:10rpx;
		width: 160rpx;
	}
	.info{
		height: 85%;
		width: 100%;
		position: absolute;
		top: 0;
		left: 200rpx;
		display: flex;
		justify-content: space-around;
		flex-direction: column;
		font-size: 25rpx;
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
