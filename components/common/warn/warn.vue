<template>
	<view class="warn-bg">
		<view class="warn">
			<text class="title">计数规则</text>
			<text class="content" style="text-align: left;margin-left: calc(750rpx * 24/ 375);">
				符合以下任意条件都可以算做久坐:

				A. 一周清醒状态下坐姿超过5天;
				B. 每天坐姿超过8小时;
				C. 经常连续1.5小时没有起身活动。
			</text>
			<text class="confirm" @click="confirmClick">知道了</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				show: false,
			}
		},
		props: {


		},
		onLoad() {

		},
		methods: {
			confirmClick() {

				this.$emit('confirmClick');
			}
		}
	}
</script>

<style>
	.warn-bg {
		width: 100%;
		height: 100vh;
		background-color: rgba(0, 0, 0, 0.5);
		overflow: hidden;
		z-index: 3;

	}

	.warn {

		overflow: hidden;
		height: calc(100vh * 227/812);
		width: calc(750rpx * 311/ 375);
		border-radius: calc(750rpx * 30/ 375);
		margin: 50% auto 0;
		margin-top: calc(100vh * 293/812);
		background-color: white;
		display: flex;
		flex-direction: column;




	}

	.warn .content {
		background-image: url(../../../static/app/white.jpg);
	}

	.warn text {
		font-size: calc(750rpx * 15/375);
	}

	.warn>text {

		text-align: center;

	}

	.warn text:nth-child(1) {
		height: calc(100vh * 55/812);
		line-height: calc(100vh * 55/812);

		margin: 0;
		padding: 0;
		font-weight: bold;


	}

	.warn text:nth-child(2) {
		width: 100%;
		height: calc(100vh * 100/812);
		text-align: center;
		margin: 0 auto;
		padding: 0;
		flex-grow: 1;


	}

	.warn text:nth-child(3) {
		height: calc(100vh * 48/812);
		line-height: calc(100vh * 48/812);
		color: rgba(19, 194, 194, 1);
		border-top: 1px solid rgba(237, 239, 242, 1);

	}
</style>
