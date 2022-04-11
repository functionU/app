<template>
	<view class="bgc">
		<view class="content">
			<tarbarHeader class="head">
				<image slot='left' style="width:calc(750rpx * 26.42/ 375);height:calc(750rpx * 28.47	/ 375);"
					src="../../static/app/back.svg" @click="backClick"></image>
				<text slot='center'
					style="display: block;text-align: center;font-size:calc(750rpx * 17/ 375);color: #FFFFFF;">修改密码</text>
				<text slot='right'></text>
			</tarbarHeader>
			<view class="main" style="background-color: #F8FDFD;">

				<view class="main-top">
					<tip :item="{name:'修改密码'}">
						<text slot="right"></text>
					</tip>
				</view>
				<view class="main-center">

					<view class="main-center-item">
						<view class="main-center-item-left">
							旧密码：
						</view>
						<view class="main-center-item-right">
							<input placeholder="请输入旧密码" @input="oldPassListen" password
								style="color: rgba(17,30,54,0.30);" />
						</view>

					</view>
					<view class="main-center-item">
						<view class="main-center-item-left">
							新密码：
						</view>
						<view class="main-center-item-right">
							<input placeholder="请输入新密码" @input=" PassLitenOne" password
								style="color: rgba(17, 30, 54, 0.3); " />
						</view>

					</view>
					<view class="main-center-item">
						<view class="main-center-item-left">
							新密码：
						</view>
						<view class="main-center-item-right">
							<input placeholder="请再次输入新密码" @input="PassLitenTwo" password
								style="color: rgba(17, 30, 54, 0.3); " />
						</view>

					</view>
				</view>
				<view class="main-bottom">
					<button type="default" @click="reviseSubmit">修改</button>
				</view>


			</view>
		</view>

	</view>

</template>\
<script>
	import tarbarHeader from '../../components/common/header/header.vue'
	import tip from '../../components/common/tip/tip.vue'


	export default {
		data() {
			return {
				oldPass: "",
				newPassOne: "",
				newPassTwo: ""
			}
		},
		onLoad() {
			if (!getApp().globalData.token) {
				uni.navigateTo({
					url: "../login/login"
				})
			}
		},
		methods: {
			backClick() {
				uni.navigateBack();
			},
			PassLitenTwo(e) {
				this.newPassTwo = e.target.value;
			},
			PassLitenOne(e) {
				this.newPassOne = e.target.value;
			},
			oldPassListen(e) {
				this.oldPass = e.target.value;
			},
			reviseSubmit() {
				if (this.newPassOne != this.newPassTwo) {
					console.log(1)
					uni.showToast({
						title: '两次新密码输入不相同',
						duration: 2000,
						icon: 'none'
					});
				} else {
					let new_password = this.newPassOne;
					let old_password = this.oldPass;
					uni.request({
						url: `http://${getApp().globalData.http}/app/user/reset/password`,
						// url: 'http://82.157.34.130:9900/app/user/reset/password',
						method: 'POST',
						data: {
							new_password,
							old_password

						},
						header: {
							'Content-Type': 'application/json',
							'Authorization': getApp().globalData.token
						},
						success: (res) => {
							if (res.data.code == 0) {
								uni.navigateBack();
							} else {
								uni.showToast({
									title: res.data.message,
									icon: 'none',
									duration: 2000
								});
							}
						}
					})

				}
			}
		},
		components: {
			tarbarHeader,
			tip,

		}
	}
</script>

<style>
	.bgc {

		height: 100vh;
		background-color: rgba(241, 242, 246, 1);
	}

	.head {
		height: calc(100vh *44/812);
	}

	.content {
		padding-top: calc(100vh * 44/812);
		height: calc(100vh * 358/812);
		background-image: url(../../static/app/bg@2X.png);
		background-size: cover;
		border-bottom-right-radius: 25%;
		border-bottom-left-radius: 25%;

	}

	.content text:nth-child(1) {
		margin-left: calc(-750rpx * 55/ 375);
	}

	.content .main {
		overflow: hidden;
		width: calc(750rpx * 343/ 375);
		height: calc(100vh * 329/812);
		border-radius: calc(750rpx * 30/ 375);
		margin-left: calc(750rpx * 16/ 375);
		margin-right: calc(750rpx * 16/ 375);
		margin-top: calc(100vh *32/812);
		background-color: rgba(255, 255, 255, 1);
		font-size: calc(750rpx * 15/ 375);
	}

	.content .main .main-top {
		height: calc(100vh *60/812);
		border-bottom: 1px solid #EDEFF2;

	}

	.content .main .main-center {


		width: calc(750rpx * 375/ 375);
		height: calc(100vh *150/812);

	}



	.content .main .main-center .main-center-item {
		width: calc(750rpx * 311/ 375);
		margin-left: calc(750rpx * 16/ 375);
		margin-right: calc(750rpx * 16/ 375);
		background-color: white;
		height: calc(100vh * 50/812);
		display: flex;
		align-items: center;

	}

	.content .main .main-center .main-center-item:nth-child(1) {

		border-bottom: 1px dashed #E6E7E8;
	}

	.content .main .main-center .main-center-item:nth-child(2) {

		height: calc(100vh * 48/812);
		border-bottom: 1px dashed #E6E7E8;

	}

	.content .main .main-center .main-center-item:nth-child(3) {
		height: calc(100vh *48/812);

		border-bottom: 1px dashed #E6E7E8;

	}

	.content .main .main-center .main-center-item .main-center-item-left {}

	.content .main .main-center .main-center-item .main-center-item-right {
		flex-grow: 1;
		font-size: calc(750rpx * 15/ 375);


	}

	.content .main .main-center .main-center-item .main-center-item-right input {

		font-size: calc(750rpx * 15/ 375);

	}

	.content .main .main-bottom {
		height: calc(100vh *105/812);

	}

	.content .main .main-bottom button {
		width: calc(750rpx * 311/ 375);
		height: calc(100vh *44/812);
		border-radius: calc(750rpx * 8/ 375);
		margin-top: calc(100vh *31/812);
		background-image: linear-gradient(135deg, rgba(112, 207, 186, 1), rgba(25, 189, 192, 1));
		;
		color: rgba(255, 255, 255, 1);
		line-height: calc(100vh *44/812);
	}
</style>
