<template>
	<view class="content" :style="{ height: wh + 'px' }">
		<image
			class="bg"
			src="../../static/images/bg.jpg"
			mode="aspectFill"
		></image>

		<image
			class="logo animate__animated"
			:class="{ animate__jello: true }"
			src="../../static/images/logo.png"
			mode=""
		></image>

		<view style="width: 600rpx;">
			<view class="input-area" style="margin-top: 50px; margin-bottom: 10px;">
				<image
					style="width: 40rpx; height: 40rpx; margin-right: 10px;"
					src="../../static/images/user.png"
					mode=""
				></image>
				<input type="text" value="" placeholder="用户名/手机号" />
			</view>

			<view class="input-area">
				<image
					src="../../static/images/mima.png"
					style="width: 40rpx; height: 40rpx; margin-right: 10px;"
					mode=""
				></image>
				<input
					:type="hidePwd ? 'password' : 'text'"
					value=""
					placeholder="密码"
				/>
				<image
					:src="`../../static/images/${hidePwd?'eye-close':'eye'}.png`"
					style="width: 40rpx; height: 40rpx; margin-left: auto;"
					mode=""
					@click="hidePwd = !hidePwd"
				></image>
			</view>

			<view
				style="background-color: orange;color:white; text-align: center; padding: 6px 0; border-radius: 5px; margin-top: 10px;"
				class="animate__animated"
				:class="{ animate__fadeIn: clickAni }"
				@click="doLogin"
			>
				<text>登录</text>
			</view>
			<view
				style="display: flex; justify-content: space-between; margin-top: 10px;"
			>
				<text style="text-decoration: underline;">忘记密码</text>
				<text style="text-decoration: underline;">立即注册</text>
			</view>
		</view>
		
		<view class="bottom">
			<view style="display: flex; width: 500rpx; align-items: center; margin-bottom: 10px;">
				<view style="background-color: white; height: 2px; flex:1"></view>
				<view style="color: white; margin: 0 5px;">第三方直接登录</view>
				<view style="background-color: white; height: 2px; flex:1"></view>
			</view>
			<view class="third-icon">
				<image src="../../static/images/qq.jpg" mode=""></image>
				<image src="../../static/images/wb.jpg" mode=""></image>
				<image src="../../static/images/wx.jpg" mode=""></image>
			</view>
		</view>
	</view>
</template>

<script>
import 'animate.css';

export default {
	data() {
		return {
			wh: uni.getSystemInfoSync().windowHeight,
			clickAni: false,
			hidePwd: true
		};
	},
	methods: {
		doLogin() {
			this.clickAni = false;

			setTimeout(() => {
				this.clickAni = true;
			}, 100);
		}
	}
};
</script>

<style lang="scss">
.content {
	display: flex;
	flex-direction: column;
	align-items: center;
}

.bg {
	position: fixed;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
	filter: blur(5px);
	z-index: -1;
}

.logo {
	background-color: white;
	width: 350rpx;
	height: 100rpx;
	border-radius: 3px;
	margin-top: 100px;
}

.input-area {
	background-color: white;
	padding: 6px;
	border-radius: 4px;
	display: flex;
	align-items: center;
}

.third-icon{
	display: flex;
	justify-content: space-around;
	width: 500rpx;
	
	image{
		width: 90rpx;
		height: 90rpx;
		border-radius: 50%;
	}
}

.bottom{
	position: absolute;
	bottom: 10px;
}
</style>
