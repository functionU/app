<template>
	<view class="bgc">
		<view class="tarbar">
			<image src="../../static/app/back.svg" style="	width: calc(750rpx *36/ 375);
		height: calc(100vh * 36/812);" @click="back"></image>
			<text>消息提醒</text>
		</view>
		<scroll-view class="content" scroll-y='true' @scrolltolower='litenLower'>
			<view class="content-item" v-for="item in warnArray">
				<view class="top-box">
					<text>{{item.reserve_station.reserve_date.split("-")[1]}}月{{item.reserve_station.reserve_date.split("-")[2]}}日{{item.reserve_station.start_time.split(":")[0] <12 ? '上午':'下午'}}{{item.reserve_station.start_time.split(":")[0]}}:{{item.reserve_station.start_time.split(":")[1]}}</text>
				</view>
				<view class="bottom-box">
					<view class="bottom-box-top">
						<view class="left">
							<image src="../../static/app/icon-xiaoxi@2X.png" style="	width: calc(750rpx * 40/ 375);
		height:calc(750rpx * 40/ 375)"></image>
						</view>
						<view class="right" style="padding-right:calc(750rpx * 5/ 375) ;">
							<text>工位签到提醒</text>
							<text>{{item.content}}</text>
						</view>

					</view>
					<view class="bottom-box-bottom">
						<text>签到时间：{{item.reserve_station.sign_time === null ?  ((item.reserve_station.reserve_date)+" "+(item.reserve_station.start_time )): item.reserve_station.sign_time}}</text>
						<text>签到地点：{{item.reserve_station.office_building_name}}—{{item.reserve_station.floor_name}}F</text>
					</view>
				</view>
			</view>

		</scroll-view>


	</view>
</template>

<script>
	export default {
		data() {
			return {
				warnArray: [],
				number: 0,
				size: 10
			}
		},
		methods: {
			back() {
				uni.navigateBack({

				})
			},
			litenLower() {
				let that = this;
				console.log(111);
				this.number++;
				uni.request({
					url: `http://${getApp().globalData.http}/app/message/list?number=${that.number}&size=${that.size}`,
					// url: `http://82.157.34.130:9901/app/message/list?number=1&size=10`,
					header: {
						'Authorization': getApp().globalData.token,
					},
					success: (res) => {

						if (res.data.code == 0) {
							that.warnArray = that.warnArray.concat(res.data.value)
						} else if (res.data.code == -100) {
							uni.showToast({
								title: '请求失败',
								duration: 2000
							});
						}

					}
				})
			}

		},

		onLoad(option) {
			let that = this;
			// if(option.count>0)
			// {
			this.number++;
			uni.request({
				url: `http://${getApp().globalData.http}/app/message/list?number=${that.number}&size=${that.size}`,
				// url: `http://82.157.34.130:9901/app/message/list?number=1&size=10`,
				header: {
					'Authorization': getApp().globalData.token,
				},
				success: (res) => {

					if (res.data.code == 0) {
						that.warnArray = res.data.value
					} else {
						uni.showToast({
							title: res.data.message,
							icon: 'none',
							duration: 2000
						});
					}

				}
			})
			// }


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
		height: calc(100vh * 33/812);
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
		line-height: calc(100vh * 21/812);
		text-align: center;
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
