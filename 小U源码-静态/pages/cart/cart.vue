<template>
	<view class="container">
		<view class="top">
			<scroll-view class="scroll-view_H" scroll-x="true" scroll-left="0" v-for="(item,index) in cartList" :key='item.id'>
				<view class="cartInfo" id="demo1">
					<!-- 购物车商品信息 -->
					<view class="cartInfochild">
						<!-- 选择框 -->
						<view class="cartInfo_switch common">
							<!-- 点击当前商品 -->
							<switch type="checkbox" :checked="item.checked" @change="singerChange($event,index)"/>
						</view>
						<!-- 图片 -->
						<view class="cartInfo_image common">
							<image :src="item.img" mode=""></image>
						</view>

						<!-- 信息 -->
						<view class="cartInfo_info">
							<label for="" style="font-size: 26rpx;">{{item.goodsname}}</label>
							<label for="" style="font-size: 24rpx; color: #ccc;">规格：黑色</label>
							<label for="" style="color: red;">￥{{item.price*item.num}}</label>
						</view>
						<!-- 加减数量 -->
						<view class="cartInfo_num">
							<view class="cartInfo_num_child">
								<label for="" @click="reduct(index)">-</label>
								<label for="">{{item.num}}</label>
								<label for="" @click="add(index)">+</label>
							</view>
						</view>
					</view>
					</view>
				<view class="cartDel" id="demo2" @click="delCart(index)"><label for="">删除</label></view>
			</scroll-view>
		</view>

		<!-- 底部 -->
		<view class="foot">
			<view class="footd1">
				<switch type="checkbox" :checked="isAllChange" @click="doAllChange"/>
				<label >全选</label>
			</view>
			<view class="footd2">
				<view class="footd2sp1">
					总计：
					<label style="color: red;">{{total}}</label>
				</view>
				<label class="footd2sp2">不含运费，已优惠￥0.00</label>
			</view>
			<!-- 跳到提交订单，结算页面 -->
			<view class="footd3" @click="goConfirm">
				<!-- 被选中的商品的个数 -->
				<label>去结算({{num}}件)</label>
			</view>
		</view>
	</view>
</template>

<script>
	import{myRequest,baseUrl}from '@/utils/db.js'
	export default {
		data() {
			return {
				cartList: [],
				
			}
		},
		methods: {
            async add(index){
				 this.cartList[index].num+=1
				this.doEdit(index)
				
			 },
			 async reduct(index){
				 if(this.cartList[index].num<=1){
					  this.cartList[index].num=1
				 }else{
				 this.cartList[index].num-=1}
				this.doEdit(index)
			 },
			 async doEdit(index){
				 let {id,num,checked}=this.cartList[index]
				 checked=checked?1:0
				  let{token}=uni.getStorageSync('userInfo')
				  var result=await myRequest('/api/cartedit',{id,num,checked},'GET',{authorization:token})
			 },
			 singerChange(e,index){
				 var value=e.detail.value
				 this.cartList[index].checked=value
				 this.doEdit(index)
			 },
			 doAllChange(){
				this.isAllChange=!this.isAllChange
				   this.cartList.forEach((item,index)=>{
					    this.doEdit(index)
				   })
			 },
			 goConfirm(){
				 var confirmList=this.cartList.filter(item=>{
					 return item.checked==true
				 })
				   uni.setStorageSync('confirmList',confirmList)
				   uni.navigateTo({
				   	url:'../confirm/confirm'
				   })
			 },
			async delCart(index){
				    let {id}=this.cartList[index]
					let{token}=uni.getStorageSync('userInfo')
					var result=await myRequest('/api/cartdelete',{id},'GET',{authorization:token})
					console.log(result)
					this.cartList.splice(index,1)
			 }
		},
		computed:{
			total(){
			var sum=0
			this.cartList.forEach(item=>{
				if(item.checked){
					sum	+=item.price*item.num
				}
			});
			return sum
			},
			num(){
				var sum=0
				this.cartList.forEach(item=>{
					if(item.checked){
						sum+=item.num
					}
				})
				return sum
			},
			isAllChange:{
				get:function(){
					return this.cartList.every(item=>{
						return item.checked==true
					})
				},
				set:function(val){
					this.cartList.forEach(item=>{
						item.checked=val
					})
				}
			}
			
		},
		async onShow() {
			let {
				uid,
				token
			} = uni.getStorageSync('userInfo')
			var resultDetail = await myRequest('/api/cartlist', {
				uid
			}, 'GET', {
				authorization:token
			})
			console.log(resultDetail)
		
			var cartList=resultDetail[1].data.list
			if(cartList){
			cartList.forEach(item=>{
				 item.img=baseUrl+item.img
				 item.checked=item.checked==1?true:false
			})
			this.cartList=cartList
			}

		}
	}
</script>

<style>
	@import url('../../common/css/cart.css');

	.cartInfo_image_img {
		width: 120rpx;
		height: 120rpx;
	}
</style>
