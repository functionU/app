<template>
	<view class="bgc">
		<view class="content">
			<tarbarHeader class="head">
				<image slot='left' style="width:calc(750rpx * 26.42/ 375);height:calc(750rpx * 28.47	/ 375);"
					src="../../static/app/back.svg" @click="backClick"></image>
				<text slot='center'>修改密码</text>
				<text slot='right'></text>
			</tarbarHeader>
			<view class="main">

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
							<input placeholder="请输入旧密码" />
						</view>

					</view>
					<view class="main-center-item">
						<view class="main-center-item-left">
							新密码：
						</view>
						<view class="main-center-item-right">
							<input placeholder="请输入新密码" />
						</view>

					</view>
					<view class="main-center-item">
						<view class="main-center-item-left">
							新密码：
						</view>
						<view class="main-center-item-right">
							<input placeholder="请输入新密码" />
						</view>

					</view>
				</view>
				<view class="main-bottom">
					<button type="default">修改</button>
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

			}
		},
		onLoad() {

		},
		methods: {
backClick(){
	uni.navigateBack();
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

		margin: 0 calc(750rpx * 16/ 375);
		width: calc(750rpx * 311/ 375);
		height: calc(100vh *150/812);

	}



	.content .main .main-center .main-center-item {

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
