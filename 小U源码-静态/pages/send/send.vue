<template>
	<view class="box">
		<view class="row">
			<text>手机号</text>
			<input type="text" v-model="phone" maxlength="12" @blur="reg"/>
		</view>
		<view class="row">
			<view class="send">
				<text>验证码</text>
				<text @click="getCode" v-if="flag">获取验证码</text>
				<text  v-else>{{time}}</text>
			</view>
			<input type="text" v-model="code" placeholder="- - - -" maxlength="4" />
		</view>
		<view class="row">
			<text style="font-size: 23rpx;">收不到验证码？试试 <text style="color:#00BB00;font-size: 23rpx;"> 语音验证</text></text>
			<!--   -->
			<button type="primary" style="width: 90%;border-radius: 80rpx;margin-top: 50rpx;" @click="goLogin" :disabled="code.length==4?false:true">登录</button>
		</view>
		<view class="row">
			<text style="color: #006699;text-align: center;">通过微信授权登录</text>
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
				phone: '',
				code: '',
				flag: true,
				time: 60,
				disabled:false
			}
		},
		methods: {
			async getCode() {
				this.flag = false
				this.time = 60
				var timer=setInterval(() => {
					this.time--
					if (this.time <= 0) {
						clearInterval(timer)
						this.flag = true
					}
				}, 1000)
				var result = await myRequest('/api/sms', {
					'phone': this.phone
				})
				console.log(result)
				var code = result[1].data.list.code
				wx.setStorageSync('code', code)

			},
			reg(){
				var reg=/^1[3456789]\d{9}$/
				 if(!(/^1[3456789]\d{9}$/.test(this.phone))){ 
				      uni.showToast({
				      	title: '手机号不正确',
				      	icon: 'none'
				      })  
				        return false; 
				    } 
			},
			async goLogin() {

				var code = this.code
				var StrongSync = uni.getStorageSync('code')
				if (code = StrongSync) {
					var result = await myRequest('/api/wxlogin', {
						'phone': this.phone
					})
					var userInfo = result[1].data.list;
					uni.setStorageSync('userInfo', userInfo)
					uni.reLaunch({
						url: '/pages/mine/mine'
					})

				} else {
					uni.showToast({
						title: '验证码不正确',
						icon: 'none'
					})
				}
			}
		},

	}
</script>

<style>
	/* 导入外部的样式文件 */
	/* @import url("../../common/css/index.css"); */
	.box {
		position: relative;
	}

	.row {
		display: flex;
		flex-direction: column;
		justify-content: space-around;
		margin: 40rpx;
	}

	.send {
		display: flex;
		justify-content: space-between;
	}

	.send text:nth-of-type(2) {
		color: #00BB00;
	}

	text {
		font-size: 27rpx;
		margin-bottom: 10rpx;
	}

	input {
		border-bottom: 1rpx solid gray;
		height: 65rpx;
	}
</style>
