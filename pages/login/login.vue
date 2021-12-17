<template>
	<view class="content">
		<view class="top">
			<view class="icon">
				<image src="../../static/app/login-logo@2X.png"></image>
			</view>
			<view class="Hello">
				<text>Hello</text>
			</view>
			<view class="text">
				<text>欢迎登录智能办公管家IoT物联平台～</text>
			</view>
		</view>
		<view class="center">
			<form class='login'>
				<view class="userName" :class="{'show-true':show,'show-false':!show}">
					<text class="account">用户名：</text>
					<input @input="inputListen" />
					<image :src=showNameImg
						style="width:calc(750rpx * 14/ 375);height:calc(750rpx * 14/ 375); margin-bottom: calc(750rpx * 17/ 375);">
					</image>
				</view>
				<view class=" userPassword">
					<view class="userPass show-true" :class="{'show-false':tag}">
						<text calss='password'>密码：</text>
						<input password @input="passwordListen" />
						<image src="../../static/app/icon-cuowu@2X.png" v-show="tag"
							style="width:calc(750rpx * 14/ 375);height:calc(750rpx * 14/ 375); margin-bottom: calc(750rpx * 17/ 375);">
						</image>
						<!-- <image  style="width:calc(750rpx * 14/ 375);height:calc(750rpx * 14/ 375);margin-bottom: calc(750rpx * 17/ 375)"></image> -->
					</view>

					<text class="loginForget" @click="forgetPass">忘记密码？</text>
				</view>


				<button class="loginButton" @click="login">登 录</button>
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
				user: "",
				pass: "",
				tag: false

			}
		},
		onLoad() {




		},
		onBackPress (){
			return true;
		},
		onShow() {



		},
		methods: {
			inputListen(e) {

				this.show = this.reg.test(e.target.value)
				this.user = e.target.value;
				if (this.show) {
					this.showNameImg = '../../static/app/icon-zhengque.png'
				} else
					this.showNameImg = '../../static/app/icon-cuowu@2X.png'
			},
			passwordListen(e) {
				this.tag = false;
				this.pass = e.target.value;
			},
			forgetPass() {
				uni.navigateTo({
					url: '../forgetPass/forgetPass'
				})
			},
			login() {
				if (this.user == "" || this.pass == "") {
                       uni.showModal({
                       	title: '提示',
                       	content: '用户名或密码不能为空',
                       	showCancel: false,
                       });

				} else {
					let name = this.user;
					let pass = this.pass;
					uni.showLoading({
						title: '加载中'
					})
					uni.request({
						url: `http://${getApp().globalData.http}/platform/app/login`,
						// url: 'http://82.157.34.130:9901/platform/app/login',
						method: 'POST',
						data: {
							password: pass,
							username: name
						},
						header: {
							'Content-Type': 'application/json'
						},
						success: (res) => {
							uni.hideLoading();
							if (res.data.code == 0) {
								getApp().globalData.token = res.data.value.token;
								uni.setStorage({
									key: "login",
									data: {
										user: name,
										password: pass
									},
									success: function() {
										console.log('success');

									}
								})
                                uni.redirectTo({
                                	url: '../login-success/login-success'
                                })
							

							} else  {
								let message=res.data.message
								uni.showToast({
									title: message,
									icon: 'none',
									duration: 2000
								});
								this.tag = true;
								this.show = false;
								this.showNameImg = '../../static/app/icon-cuowu@2X.png'
							}



						}
					})
				}



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
	}

	.top {
		margin-top: calc(100vh * 114 /812);
		margin-left: calc(750rpx * 32/ 375);

	}

	.top .icon image {
		width: calc(750rpx * 32/ 375);
		height: calc(100vh * 32/812);
	}

	.top .Hello {
		display: inline-block;
		font-size: calc(750rpx * 30/ 375);
		font-weight: bold;
		line-height: calc(100vh * 42/812);
		margin-top: calc(100vh * 14/812);
		-webkit-background-clip: text;
		color: transparent;
		background-image: linear-gradient(to right, rgba(112, 207, 186, 1), rgba(25, 189, 192, 1));

	}

	.top .text {
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
