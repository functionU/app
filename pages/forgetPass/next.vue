<template>
	<view class="content">
		<view class="tarbar">
			<text @click="back">取消</text>
		</view>
		<view class="top">
			<image src="../../static/app/Sign.png" style="width: calc(750rpx * 80/ 375);
		height: calc(750rpx * 80/ 375;"></image>
		<text>发送成功</text>
		<text>请登录您的邮箱，查看重置密码链接。</text>
		</view>
		<view class="center">
			<form class='login'>
				<button class="loginButton" @click="next">立即登录</button>
			</form>
		</view>


	</view>
</template>

<script>
	export default {
		data() {
			return {
				show: true,
				reg: /^[a-zA-Z0-9_-]+@[a-zA-Z0-9_-]+(\.[a-zA-Z0-9_-]+)+$/,
				showNameImg: "",
				email: "",
			}
		},

		methods: {
			inputListen(e) {

				this.show = this.reg.test(e.target.value)
				this.email = e.target.value;
				if (this.show) {
					this.showNameImg = '../../static/app/icon-zhengque.png'
				} else
					this.showNameImg = '../../static/app/icon-cuowu@2X.png'

			},
			next() {
				uni.navigateTo({
					url: '../login/login'
				})
			},
			back(){
				uni.navigateTo({
					url: '../login/login'
				})
			}


		}
	}
</script>

<style>
	.content {
		width: 100vw;
		height: 100vh;
		overflow: hidden;
		font-size: calc(750rpx * 15 / 375);
		background-image: url(../../static/app/bg-left.svg);
		background-repeat: no-repeat;
		background-position: top right;
		
	}

	.tarbar {
		margin-top: calc(100vh * 44/812);
		width: calc(750rpx * 375 / 375);
		height: calc(100vh * 44/812);
	}

	.tarbar text {
		margin-left: calc(750rpx * 32/ 375);
	}

	.top {
		margin-top: calc(100vh * 38 /812);
	
       display: flex;
	   flex-direction: column;
	   align-items: center;
	}
.top text{
	text-align: center;
}

	.center {
		margin-top: calc(100vh * 50/812);
	}

	.login {
		color: rgba(10, 32, 57, 0.5);
	}

	.login .loginButton {

		color: rgba(255, 255, 255, 1);
		width: calc(750rpx * 327 / 375);
		height: calc(100vh * 56/812);
		line-height: calc(100vh * 56/812);
		margin-top: calc(100vh * 50/812);
		background-image: linear-gradient(to right, rgba(112, 207, 186, 1), rgba(25, 189, 192, 1));
		border-radius: calc(750rpx * 8/ 375);
	}

	.show-true {
		border-bottom: calc(750rpx * 1/ 375) solid #13C2C2;
	}

	.show-false {
		border-bottom: calc(750rpx * 1/ 375) solid #FB696C;
	}
</style>
