<template>
	<view class="content">
		<view class="tarbar">
			<text @click="back">取消</text>
		</view>
		<view class="top" >
			<view class="text">
				<text>通过邮箱找回密码</text>
			</view>
		</view>
		<view class="center" >
			<form class='login'>
				<view class="userName" :class="{'show-true':show,'show-false':!show}">
					<text class="account">邮箱：</text>
					<input @input="inputListen" placeholder="请输入注册时填写的邮箱" />
					<image :src=showNameImg
						style="width:calc(750rpx * 14/ 375);height:calc(750rpx * 14/ 375); margin-bottom: calc(750rpx * 17/ 375);">
					</image>
				</view>
				<button class="loginButton" @click="next">下一步</button>
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
				let email = this.email;

				uni.request({
					url: `http://${getApp().globalData.http}/app/user/forget/password`,
					// url: 'http://82.157.34.130:9901/app/user/forget/password',
					method: 'POST',
					data: {
						email: email
					},
					header: {
						'Content-Type': 'application/json'
					},
					success: (res) => {

						if (res.data.code == 0) {
							uni.navigateTo({
								url: '../forgetPass/next'
							})
						} else {
							uni.showToast({
								title: res.data.message,
								icon: 'none',
								duration: 2000
							});
						}

					}
				})

			},
			back() {
				uni.navigateBack({

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
		font-size: calc(750rpx * 14/ 375);
		color: rgba(10, 32, 57, 0.7);
	}

	.top {
		margin-top: calc(100vh * 94 /812);
		margin-left: calc(750rpx * 24/ 375);

	}


	.top .text {
		font-size: calc(750rpx * 24 / 375);
		font-weight: bold;
		line-height: calc(100vh * 21/812);
		color: rgba(10, 32, 57, 0.7);
		margin-top: calc(100vh * 13/812);

	}

	.center {
		margin-top: calc(100vh * 72/812);
	}

	.login {
		color: rgba(10, 32, 57, 0.5);
	}


	.login .userName {
		margin-left: calc(750rpx * 24/ 375);
		margin-right: calc(750rpx * 23/ 375);

	}

	.login .userName .account {
		width: calc(750rpx * 24 / 375);
		height: calc(100vh * 23/812);
	}

	.login .userName input {
		display: inline-block;
		width: calc(750rpx * 313/ 375);
		margin-bottom: calc(100vh * 13/812);
		margin-top: calc(100vh * 13/812);

	}

	.login .userPassword {

		width: calc(750rpx * 328 / 375);
		height: calc(100vh * 103/812);
		margin-left: calc(750rpx * 24/ 375);
		margin-right: calc(750rpx * 23/ 375);
		margin-top: calc(100vh * 23/812);
	}

	.login .userPassword input {
		display: inline-block;
		width: calc(750rpx * 313/ 375);
		margin-top: calc(100vh * 13/812);
		margin-bottom: calc(100vh * 13/812);

	}

	.login .userPassword .userPass {}

	.login .loginForget {
		display: block;

		width: calc(750rpx * 75 / 375);
		height: calc(100vh * 21/812);
		margin-top: calc(100vh * 13/812);
		margin-left: calc(750rpx * 252/ 375);

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
