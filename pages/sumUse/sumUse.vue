<template>
	<view class="bgc">
		<view class="content">
			<tarbarHeader class="head">
				<image slot='left' style="width:calc(750rpx * 26.42/ 375);height:calc(750rpx * 28.47/375);"
					src="../../static/app/back.svg" @click="backClick"></image>
				<text slot='center'
					style="display: block;text-align: center;font-size:calc(750rpx * 17/ 375);color: #FFFFFF;">用电统计</text>
				<text slot='right'></text>
			</tarbarHeader>
			<view class="top">
				<view class="left" @click="reserveClick">
					<image src="../../static/app/yuyue@2X.png" mode=""></image>
				</view>
				<view class="right">
					<text>今天总共用了<b style="font-size:calc(750rpx * 20/ 375) ;">{{item.text}}</b>kw·h 电量</text>
					<text>注意节约用电哦～</text>
				</view>
			</view>
			<view class="bottom">
				<qiun-data-charts type="bar" :chartData="envirChartData" background="none" />
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
				item: {

					text: "15.6"

				},
				envirChartData: {


					categories: ['周一', '周二',
						'周三', '周四', '周五'
					],
					series: [{
						name: '指数',
						data: [0, 0, 0, 0, 0],
						color: '#70CFBA'
					}]
				}
			}
		},
		onLoad() {

			new Promise(function(resolve, reject) {
				uni.request({
					url: `http://${getApp().globalData.http}/app/data/today/power`,
					header: {
						'Authorization': getApp().globalData.token,
					},
					success: (res) => {
						// uni.showLoading({
						// 	title: '加载中'
						// })

						resolve(res.data.value);
					}
				})
			}).then(res => {
				this.item.text = res;
				return new Promise(function(resolve, reject) {

					uni.request({
						url: `http://${getApp().globalData.http}/app/data/fix/power/today/statistics`,
						header: {
							'Authorization': getApp().globalData.token,
						},
						success: (res) => {

							console.log(res)
							// uni.hideLoading();
							resolve(res.data.value);



						}
					})
				})
			}).then(res => {
				console.log(res);
				let x = [];
				let y = [];
				res.map((item) => {
					x.push(item.x_value);
					y.push(item.y_value);
				})
				console.log(x);
				console.log(y);
				this.envirChartData = {
					categories: [...x],
					series: [{
						name: '指数',
						data: [...y],
						color: '#70CFBA'
					}, ]
				};

			})

		},
		props: {

		},
		methods: {
			backClick() {
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

	.content .head text:nth-child(1) {
		margin-left: calc(-750rpx * 55/ 375);
	}

	.content .top {
		overflow: hidden;
		width: calc(750rpx * 343/ 375);
		height: calc(100vh * 92/812);
		border-radius: calc(750rpx * 30/ 375);
		margin-left: calc(750rpx * 16/ 375);
		margin-right: calc(750rpx * 16/ 375);
		margin-top: calc(100vh * 16/812);
		background-color: rgba(10, 32, 57, 0.1);
	}

	.top {
		margin-top: calc(100vh * 20/812);
		background-color: rgba(10, 32, 57, 0.1);
		display: flex;

		align-items: center;
	}

	.top .left image {
		width: calc(750rpx * 60/ 375);
		height: calc(100vh * 60/812);
		margin-left: calc(750rpx * 24/ 375);
	}

	.top .right text {
		color: rgba(255, 255, 255, 1);
		font-size: calc(750rpx * 14/ 375);
		display: block;
		margin-left: calc(750rpx * 20/ 375);
	}

	.bottom {
		overflow: hidden;
		width: calc(750rpx * 343/ 375);
		height: calc(100vh * 538/812);
		border-radius: calc(750rpx * 30/ 375);
		margin-left: calc(750rpx * 16/ 375);
		margin-right: calc(750rpx * 16/ 375);
		margin-top: calc(100vh * 20/812);
		background-color: rgba(255, 255, 255, 1);
	}
</style>
