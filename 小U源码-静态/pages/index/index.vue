<template>
	<view class="">
		<!-- 顶部信息 -->
		<view class="HomeTop">
		  <view class="logoView">
			<!-- logo -->
			<img class="logo" src="/static/index/logo.jpg" alt="" />
		  </view>
		  <view class="inputView">
			<input class="textinput" style="center" type="text"   placeholder="寻找商品"  @input='changeValue'/>
			
		  </view><text class="shousuo" @click="goSearch" >搜索</text>
		</view>
		
		<!-- 顶部导航  scroll-view -->
		<view class="tabs">
		    <scroll-view id="tab-bar" class="scroll-h" :scroll-x="true" :show-scrollbar="false">
		        <view class="uni-tab-item"  data-current="0"  v-for="(item,index) in list" :key='item.id' @click="goProduct(item.id)">
		            <text :class="active==index?'uni-tab-item-title-active  uni-tab-item-title':'uni-tab-item-title' " @click="activeClass(index)">
						{{item.catename}}
					</text>
		        </view>
		    </scroll-view> 
		</view>
		<!-- 推荐轮播图 -->
		<view class="">
			<swiper :indicator-dots="true" :autoplay="true" :interval="2000" :duration="1000">
				<swiper-item v-for="(item,index) in banner" :key='item.id'>
					<view class="swiper-item" >
						<image :src="item.img" mode=""></image>
					</view>
				</swiper-item>
				<!-- <swiper-item >
					<view class="swiper-item">
						<image src="../../static/index/swiper.jpg" mode=""></image>
					</view>
				</swiper-item>
				<swiper-item >
					<view class="swiper-item">
						<image src="../../static/index/swiper.jpg" mode=""></image>
					</view>
				</swiper-item> -->
				
			</swiper>
		</view>
		
		<!-- 功能导航开始 -->
		<!-- 功能导航 -->
		<view class="FunctNavCon">
		  <view class="FunctNavLi">
			<image src="../../static/index/xiaohuoban.png" alt class="FunImg" />
			<p>限时抢购</p>
		  </view>
		  <view class="FunctNavLi">
		  			<image src="../../static/index/jifentixicopy.png" alt class="FunImg" />
		  			<p>积分商城</p>
		  </view>
		  <view class="FunctNavLi">
		  			<image src="../../static/index/lianxiwomen.png" alt class="FunImg" />
		  			<p>联系我们</p>
		  </view>
		  <view class="FunctNavLi" @click="goClassify">
		  			<image src="../../static/index/-shangpinfenlei-gai.png" alt class="FunImg" />
		  			<p>商品分类</p>
		  </view>
		</view>
		
		<!-- 推荐部分 未写 -->
		<view class="HotGoods">
			<!-- 热推左侧 -->
			<view class="HotLeft">
				<view class="LimitedContent">
				  <image class="LimitedImg" src="../../static/index/xianshi.jpg" alt />
				  <label class="Limited">限时秒杀</label>
				</view>
				<p class="LimitedTitle">每天零点场，好货秒不停</p>
				<!-- 倒计时 -->
				<view class="LimitTimeAll">
				  <label class="LimitTime">{{hours}}</label>
				  <view class="maohao">:</view>
				  <label class="LimitTime">{{minute}}</label>
				  <view class="maohao">:</view>
				  <label class="LimitTime">{{second}}</label>
				  <label class="Seckill">秒杀</label>
				</view>
				<image :src="seckillImg" class="LimitedgoodsImg" alt="">
			</view>
			
			<!-- 热推右侧 -->
			<view class="HotRight">
				<!-- 右侧顶部部分 -->
				<view class="HotTop">
					<view class="HomeTopInfo">
						<view class="HomeTopInfoOne">
							<label class="BrandNew">品牌上新</label>
							<label class="Discount">折</label>
						</view>
						<view class="HomeTopRobbig">
							<label>每日九点，抢大牌</label>
						</view>
					</view>		
					<!-- <div> -->
					<view class="Cloth">
						<image  src="../../static/index/goods1.jpg" alt="">
					</view>
					
					<!-- </div> -->				
				</view>
				<!-- 右侧底部 -->
				<view class="HotBottom">
					<view class="HotBottomLeft">
						<view class="HotBottomLeftInfo">
							<label class="HotBottomLeftInfoTitle">每日十件</label>
							<label class="HotBottomLeftInfoDetail">只为你选好货</label>
						</view>
						<view class="HotBottomLeftImage">
							<image src="../../static/index/goods1.jpg" mode=""></image>
						</view>
					</view>
					<view class="HotBottomRight">
						<view class="HotBottomRightInfo">
							<label class="HotBottomRightInfoTitle">每日十件</label>
							<label class="HotBottomRightInfoDetail">只为你选好货</label>
						</view>
						<view class="HotBottomRightImage">
							<image src="../../static/index/goods1.jpg" mode=""></image>
						</view>
					</view>
				</view>
			</view>
		</view>
		  <!-- 轮播图2 -->
		<div class="Swiper2">
		  <image class="Swiper2Img" src="../../static/index/swiper1.jpg" alt />
		</div>
		
		<!-- 底部商品列表 -->
		<view class="products">
			<!-- 商品标签 -->
			<view class="tags">
				<view  @click="flag=0" :class="flag==0?'active_tag_list tag_list':'tag_list'" >
					<label for="">热门推荐</label>
				</view>
				<view class="tag_list" @click="flag=1" :class="flag==1?'active_tag_list tag_list':'tag_list'">
					<label for="" >发现好货</label>
				</view>
				<view class="tag_list" @click="flag=2" :class="flag==2?'active_tag_list tag_list':'tag_list'">
					<label for="">只看专场</label>
				</view>
			</view>
			<!-- 标签对应的商品 -->
			
			<view class="tags_product" v-for="(item,index) in goodsList" :key='index' v-show="flag==index">
				<view class="product" v-for="(m,i) in item.content" :key='m.id' @click="goDetail(m.id)">
					<view class="GoodsLeft">
						<image class="GoodsImg" :src="m.img" alt />
					</view>
					<view class="GoodsCont">
						<view class="GoodConTit">{{m.goodsname}}</view>
						<view class="GoodsPrice">￥{{m.market_price}}</view>
						<view class="yimai">已售8000件</view>
						<view class="Immed">立即抢购</view>
					</view>
				</view>
				
			
			</view>
		</view>
	</view>
</template>

<script>
	import{myRequest,baseUrl}from'@/utils/db.js'
	export default{
		 data(){
			 return{
				 list:[],
				 banner:[],
				 seckill:[],
				 active:0,
				 hours:'',
				 minute:'',
				 second:'',
				 seckillImg:'',
				 flag:0,
				 goodsList:[],
				 keywords:''
				 
			 }
		 },
		 methods:{
			 activeClass(index){
				 this.active=index
			 },
			 goDetail(id){
				 uni.navigateTo({
				 url:'/pages/details/details?id='+id,
				 })
			 },
			 goProduct(id){
	
				 uni.navigateTo({
				 url:'/pages/product/product?id='+id,
				 })
			 },
			 changeValue(event){
				
				 this.keywords=event.detail.value
			 },
			 goSearch(){
				 console.log(this.keywords)
				uni.navigateTo({
				url:'/pages/search/search?keywords='+this.keywords,
				})
			 },
			 goClassify(){
				 uni.navigateTo({
				 url:'/pages/classify/classify'
				 })
			 }
			 
		 },
	async onLoad(){
			// 获取一级分类信息
		var resultHeader=await myRequest("/api/getcate")
		this.list=resultHeader[1].data.list
		console.log(resultHeader)
		// 获取轮播图信息
		var resultBanner=await myRequest("/api/getbanner")
		
		this.banner=resultBanner[1].data.list
		  this.banner.forEach(item=>{
			  item.img=baseUrl+item.img
		  })
		  // 获取限时秒杀信息
		var resultSeckill=await myRequest("/api/getseckill")
		
		this.seckill=resultSeckill[1].data.list
		  var endTime=this.seckill[0].endtime
this.seckillImg=baseUrl+this.seckill[0].img
		var timer=  setInterval(()=>{
		  var nowTime=new Date().getTime()
		  var distance=Math.floor((endTime-nowTime)/1000)
		  var hours=Math.floor(distance/3600)
		  var minute=Math.floor(distance%3600/60)
		   var second=Math.floor(distance%3600%60)
		  this.minute=minute<10?'0'+minute:minute
		   this.hours=hours<10?'0'+hours:hours
		    this.second=second<10?'0'+second:second
			if(distance==0){
				clearInterval(timer)
			}
		        },1000)	
	// 获取商品信息
	var resultGoods=await myRequest("/api/getindexgoods")
	console.log(resultGoods)
	 this.goodsList=resultGoods[1].data.list
	 	this.goodsList.forEach(item=>{
		return item.content.forEach(i=>{
		 return	i.img=baseUrl+i.img
		})
		})
		}
		
	}
</script>

<style>
	/* 导入外部的样式文件 */
	@import url("../../common/css/index.css");
	.shousuo{
		float:right
	}
</style>
