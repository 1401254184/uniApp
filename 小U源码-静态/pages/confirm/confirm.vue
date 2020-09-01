<template>
	<view class="container">
		<!-- s收货地址 -->
		<view class="address">
			<view class="address_info">
				<label for="">收件人:{{username}}</label>
				<label for="">{{userphone}}</label>
			</view>
			<view class="address_res">
				收货地址：<label for="">{{address}}</label>
			</view>
		</view>

		<!-- 商品信息 -->
		<view class="carts">


			<view class="cartsInfo" v-for="(item,index) in confirmList" :key='item.index'>
				<!-- 70px -->
				<view class="carts_detail">
					<view class="carts_image">
						<image :src="item.img" mode=""></image>
					</view>
					<view class="carts_name">
						<view for="" style="font-size: 30rpx;">{{item.goodsname}}</view>
						<view for="" style="font-size: 26rpx; color: #C0C0C0;">规格:300</view>
					</view>
					<view class="carts_price">
						<label for="">￥ {{item.price}}</label>
					</view>
				</view>
				<!-- 50px -->
				<view class="cart_num">
					<view class="">
						购买数量：
					</view>
					<view class="">
						<label for="" class="jian" @click="reduct(index)">-</label>
						<label for="" class="num">{{item.num}}</label>
						<label for="" class="jia" @click="add(index)">+</label>
					</view>
				</view>
				<!-- 横线 -->
				<view class="xian"></view>
			</view>



			<!-- 50px -->
			<view class="kuaidi">
				<label for="">配送方式</label>
				<label for="">XX快递</label>
			</view>
		</view>

		<!-- 优惠券 -->
		<view class="yhquan">
			<label for="">优惠券</label>
			<label for="" style="margin-right: 40rpx;">无可用</label>
		</view>
		<!-- jifen积分 -->
		<view class="jifen">
			<view class="use">
				<label for="">使用积分</label>
			</view>
			<view class="input" style="margin-right: 40rpx;">
				<input type="text" value="" placeholder="输入积分" />
				<label for="" class="input_use">使用</label>
				<label for="" class="input_keyi">可使用50积分</label>
			</view>
		</view>

		<!-- 具体详细 -->
		<view class="resDetail">
			<view class="">
				<label for="">商品金额</label>
				<label for="" style="margin-right:40rpx;color:red">￥{{total}}</label>
			</view>
			<view class="">
				<label for="">运费</label>
				<label for="" style="margin-right:40rpx;color:red">+￥0.00</label>
			</view>
			<view class="">
				<label for="">优惠券</label>
				<label for="" style="margin-right:40rpx;color:red">-￥0.00</label>
			</view>
			<view class="">
				<label for="">会员优惠</label>
				<label for="" style="margin-right:40rpx;color:red">-￥0.00</label>
			</view>
			<view class="">
				<label for="">积分抵扣</label>
				<label for="" style="margin-right:40rpx;color:red">-￥0.00</label>
			</view>
		</view>
		<!-- 实付金额 -->
		<view class="sfje">
			实付金额：<label for="" style="color:red">￥{{ total }}</label>
		</view>

		<!-- 确认订单按钮 -->
		<view class="btn">
			<!-- 点击提交订单按钮，发起支付请求 -->
			<button type="primary" @click="pay">提交订单</button>
		</view>
	</view>
</template>

<script>
	import {
		myRequest,
		baseUrl
	} from '@/utils/db.js'
	export default {
		data() {
			return {
				confirmList: [],
				username: '邹',
				userphone: '18380251257',
				address: '四川省成都市'
			}
		},
		methods: {
			async add(index) {
				this.confirmList[index].num += 1
				 uni.setStorageSync('confirmList',this.confirmList)

			},
			async reduct(index) {
				if (this.confirmList[index].num <= 1) {
					this.confirmList[index].num = 1
				} else {
					this.confirmList[index].num -= 1
				}
				// this.doEdit(index)
				 uni.setStorageSync('confirmList',this.confirmList)
			},
			
		async	pay() {
				var idstr = this.confirmList.map(item => {
					return item.id
				})
				idstr=idstr.join(',')
				// idstr = JSON.stringify(idstr)
					var data = {
						uid: this.confirmList[0].uid,
						username: this.username,
						userphone: this.userphone,
						address: this.address,
						countmoney: this.total,
						countnumber: this.countnumber,
						addtime: new Date().getTime(),
					}
					data = JSON.stringify(data)
					let {
						token
					} = uni.getStorageSync('userInfo')
					console.log(data,idstr)
	      	var result = await myRequest('/api/orderadd', {params: data,idstr: idstr}, 'GET', {authorization: token})
					console.log(result)
					uni.navigateTo({
						url:'../order/order'
					})
			}
		},
		computed: {
			total() {
				var sum = 0
				this.confirmList.forEach(item => {
					sum += item.price * item.num
				})
				return sum
			},
			countnumber() {
				var sum = 0
				this.confirmList.forEach(item => {
					sum += item.num
				})
				return sum
			}
		},

		async onLoad() {
			this.confirmList = uni.getStorageSync('confirmList')||[]

		}
	}
</script>
<style>
	@import url("../../common/css/confirm.css");
</style>
