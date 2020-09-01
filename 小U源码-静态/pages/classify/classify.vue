<template>
	<view class="container">
		<!-- left左侧列表 -->
		<view class="left">
			<!-- 循环遍历的 -->
			<view :class="active==index?'activeList  left_list':'left_list'"  v-for="(item,index) in classifyList" :key='item.id' @click="changeList(index)">
				<label for="">{{item.catename}}</label>
			</view>
			<!-- <view class="left_list" >
				<label for="">沙宣</label>
			</view>
			<view class="left_list" >
				<label for="">施华蔻</label>
			</view>
			<view class="left_list" >
				<label for="">欧莱雅</label>
			</view>
			<view class="left_list" >
				<label for="">施华蔻</label>
			</view>
			<view class="left_list" >
				<label for="">施华蔻</label>
			</view> -->
		</view>
		<!-- right右侧详细分类商品 -->
		<view class="right">
			<!-- 每一个小类 -->
			<view class="right_list">
				<!-- 商品 -->
				<view class="bottom" >
					<view class="bottom_list"  v-for="(item,index) in classifyList[active].children" :key='item.id' @click="goProduct(item.id)">
						<!-- 每个最多显示6个 -->
						<view >
							<image :src="item.img" alt="">
						</view>
						<view class="title">
							<span>{{item.catename}}</span>
						</view>
					</view>
				</view>
				
			</view>
		</view>
	</view>
</template>

<script>import {
		myRequest,
		baseUrl
	} from '@/utils/db.js'
	export default{
		data(){
			return{
		      classifyList:[],
			  active:0
			}
		},
		methods:{
	changeList(index){
		this.active=index
	},
	goProduct(id){
		
					 uni.navigateTo({
					 url:'/pages/product/product?id='+id,
					 })
	},
		},
		async onLoad() {
			
			var resultDetail = await myRequest('/api/getcates')
	
		    console.log( resultDetail[1].data.list)
			
		   resultDetail[1].data.list.forEach(item=>{
			   console.log( item)
			   if(item.children){
				item.children.forEach( m=>{
					   m.img=baseUrl+m.img
				})}else{
					item.children=[]
				}
		   })
		    this.classifyList=resultDetail[1].data.list
			console.log(this.classifyList)
		 }
	}
</script>
<style>
	/* 导入外部的样式文件 */
	@import url("../../common/css/classify.css");
	
	/* 点击左侧导航，显示动态样式 */
	.activeList{
		border-left: 6rpx solid #f26b11;
		color: #f26b11;
	}
</style>
