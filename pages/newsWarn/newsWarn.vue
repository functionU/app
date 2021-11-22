<template>
	<view class="bgc">
		<view class="tarbar">
			<image src="../../static/app/back.svg" style="	width: calc(750rpx *36/ 375);
		height: calc(100vh * 36/812);" @click="back"></image>
			<text>消息提醒</text>
		</view>
		<view class="content">
			<view class="content-item" v-for="item in warnArray">
				<view class="top-box">
					<text>{{item.month}}月{{item.day}}日 {{item.hours <12 ? '上午':'下午'}}{{item.hours}}:{{item.mins}}</text>
				</view>
				<view class="bottom-box">
					<view class="bottom-box-top">
						<view class="left">
							<image src="../../static/app/icon-xiaoxi@2X.png" style="	width: calc(750rpx * 40/ 375);
		height:calc(750rpx * 40/ 375)"></image>
						</view>
						<view class="right">
							<text>工位签到提醒</text>
							<text>您好，您预订的工位{{item.position}}即将开始
								使用，请尽快进行签到。</text>
						</view>

					</view>
					<view class="bottom-box-bottom">
						<text>签到时间：{{item.year}}-{{item.month}}-{{item.day}} {{item.hours}}:{{item.mins}}</text>
						<text>签到地点：{{item.plcae}}—{{item.floor}}F</text>
					</view>
				</view>
			</view>

		</view>


	</view>
</template>

<script>
	export default {
		data() {
			return {
				warnArray: []
			}
		},
		methods: {
			back() {
				uni.navigateBack({

				})
			}
		},
		onLoad() {
			let that = this;
			uni.request({
				url: `http://${getApp().globalData.http}/app/message/list?number=1&size=10`,
				// url: `http://82.157.34.130:9901/app/message/list?number=1&size=10`,
				header: {
					'Authorization': getApp().globalData.token,
				},
				success: (res) => {

					that.warnArray = res.data.value
				}
			})

		},
	}
</script>

<style>
	.bgc {
		overflow: hidden;
		height: 100vh;
		background-image: url(../../static/app/bg@2X.png);
		background-position: 50% 0;
	}

	.bgc .tarbar {
		=height: calc(100vh * 33/812);
		line-height: calc(100vh * 44/812);
		margin-top: calc(100vh * 44/812);
		padding-bottom: calc(100vh * 11/812);
		display: flex;
		align-items: center;


	}

	.bgc .tarbar image {}

	.bgc .tarbar text {
		margin-left: 30%;
		font-size: calc(750rpx * 17/ 375);
		color: rgba(255, 255, 255, 1);
	}

	.bgc .content {
		overflow-y: scroll;
		width: calc(750rpx * 375/ 375);
		height: calc(100vh * 724/812);

		background-color: rgba(241, 242, 246, 1);
	}

	.bgc .content .content-item {

		margin-top: calc(100vh * 20/812);
		margin-left: calc(750rpx * 16/ 375);
		width: calc(750rpx * 343/ 375);
		height: calc(100vh * 245/812);

		display: flex;
		flex-direction: column;
		align-items: center;

	}

	.bgc .content .content-item .top-box {
		width: calc(750rpx * 116/ 375);
		height: calc(100vh * 21/812);
		background-color: white;
		border-radius: calc(750rpx * 4/ 375);
		font-size: calc(750rpx * 11/ 375);
	}

	.bgc .content .content-item .bottom-box {
		margin-top: calc(100vh * 20/812);
		width: calc(750rpx * 343/ 375);
		height: calc(100vh * 204/812);
		background-color: rgba(255, 255, 255, 1);
		border-radius: calc(750rpx * 30/ 375);
	}

	.bgc .content .content-item .bottom-box .bottom-box-top {
		display: flex;

	}

	.bgc .content .content-item .bottom-box .bottom-box-top .left {
		margin-top: calc(100vh * 27/812);

		margin-left: calc(750rpx * 32/ 375);

	}


	.bgc .content .content-item .bottom-box .bottom-box-top .right {
		margin-left: calc(750rpx * 16/ 375);
		display: flex;
		flex-direction: column;



	}

	.bgc .content .content-item .bottom-box .bottom-box-top .right text:nth-child(1) {
		margin-top: calc(100vh * 24/812);
		font-size: calc(750rpx * 18/ 375);
		font-weight: bold;
	}

	.bgc .content .content-item .bottom-box .bottom-box-top .right text:nth-child(2) {
		margin-top: calc(100vh * 12/812);
		font-size: calc(750rpx * 14/ 375);
		margin-bottom: calc(100vh * 12/812);


	}

	.bgc .content .content-item .bottom-box .bottom-box-bottom {
		width: calc(750rpx * 295/ 375);

		display: flex;
		flex-direction: column;
		margin-left: calc(750rpx * 27/ 375);
		border-top: dashed calc(750rpx * 1/ 375) rgba(230, 231, 232, 1);
		padding-top: calc(100vh *16/812);
		font-size: calc(750rpx * 12/ 375);
		color: rgba(10, 32, 57, 0.5);
	}

	.bgc .content .content-item .bottom-box .bottom-box-bottom text:first-child {
		margin-bottom: calc(100vh * 12/812);
	}
</style>
