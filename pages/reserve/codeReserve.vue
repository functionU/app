<template>
	<view class="bgc">
		<view class="content">
			<tarbarHeader class="head">
				<image slot='left' style="width:calc(750rpx * 26.42/ 375);height:calc(750rpx * 28.47/ 375);"
					src="../../static/app/cancal.svg" @click="backClick"></image>
				<text slot='center'></text>
				<text slot='right'
					style="font-size:calc(750rpx * 17/ 375) ; color: white;margin-left: calc(-750rpx * 220/ 375);">选择时长</text>
			</tarbarHeader>
			<view class="top">

				<view class="top-item" style="display: flex;justify-content: space-around;align-items: center;">
					<view style="display: flex;flex-direction: column;justify-content: center;">
						<text>您选择工位为:</text>
						<text style="font-size: calc(750rpx * 30/ 375);">{{position}}</text>
					</view>
					<view>
						<image src="../../static/app/yuyue@2X.png"
							style="width:calc(750rpx * 60/375);height:calc(750rpx * 60/375); transform: translateY(5%);">
						</image>
					</view>

				</view>
			</view>
			<view class="center">
				<view class="center-head">

					<text>使用时长 </text>

				</view>
				<view class="center-boxOne">
					<view class="center-boxOne-top">
						<view class="center-boxOne-top-content">
							<view @click="itemClick(item,index)" :class="{'click':index==itemIndex}"
								class="center-boxOne-top-content-item" v-for="(item,index) in positionArray">
								{{item}}{{item < 30 ? '小时':'分钟'}}
							</view>
						</view>
					</view>
					<view class="center-boxOne-bottom">
						<tip :item="{name:'使用时间	'}" class="tip">
							<text slot="right"></text>
						</tip>
						<view class="center-boxOne-bottom-centent" style="display: flex;justify-content: space-around;">
							<view>
								<text>{{startTime}}</text>
								<text style="font-size:calc(750rpx * 14/ 375);color: gray;">（当前时间）</text>
							</view>

							<view>
								<text>{{endTime}}</text>
								<text style="font-size:calc(750rpx * 14/ 375);color: gray;">（结束）</text>
							</view>

						</view>
					</view>
				</view>

			</view>
			<view class="bottom">
				<view @click="reseverFinshed" class="resever">
					立即预订
				</view>
				<view @click="backClick" class="cancal">
					以后再说
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
			const date = new Date();
			let hour = date.getHours() < 10 ? '0' + date.getHours() : date.getHours();
			let mins = date.getMinutes() < 10 ? '0' + date.getMinutes() : date.getMinutes();
			let year = date.getFullYear();
			let month = (date.getMonth() + 1) < 10 ? '0' + (date.getMonth() + 1) : (date.getMonth() + 1);
			let day = date.getDate() < 10 ? '0' + date.getDate() : date.getDate();
			let that = this;


			return {
				startTime: hour + ":" + mins,
				endTime: hour + ":" + mins,
				itemIndex: -1,
				show: true,
				position: 'YJ630',
				tag: false,
				now: year + "-" + month + "-" + day,
				id: -1,

			}

		},
		props: {
			positionArray: {
				type: Array,
				default: () => {
					return [30, 1, 2, 4, 6, 8]
				}
			},

		},
		onLoad(option) {
			if (option) {
				this.position = option.station_number;
				this.id = option.id;

			}


		},

		methods: {

			backClick() {
				uni.navigateBack({

				})
			},
			itemClick(item, index) {
				this.itemIndex = index;
				let timeString = this.startTime.split(":");
				let hour = parseInt(timeString[0]);
				let min = parseInt(timeString[1]);
				if (item < 30) {
					hour += item;
					if (hour >= 24) {
						hour -= 24;
						this.tag = true;

					}
				} else {
					min = min + 30;
					if (min >= 60) {
						hour += 1;
						min = min - 60;

					}
				}
				hour = hour < 10 ? '0' + hour : hour;
				min = min < 10 ? '0' + min : min;
				this.endTime = `${hour}:${min}`

			},
			reseverFinshed() {


				if (this.itemIndex < 0) {
					uni.showModal({
						title: '提示',
						content: '请选择使用时长',
						showCancel: false,
					});
				} else {
                    let that=this;
					let date = new Date();
					let hour = date.getHours() < 10 ? '0' + date.getHours() : date.getHours();
					let mins = date.getMinutes() < 10 ? '0' + date.getMinutes() : date.getMinutes();
					let year = date.getFullYear();
					let month = (date.getMonth() + 1) < 10 ? '0' + (date.getMonth() + 1) : (date.getMonth() + 1);
			     	let day = date.getDate() < 10 ? '0' + date.getDate() : date.getDate();
					let startTime = `${hour}:${mins}:00`;
					let now = year + "-" + month + "-" + day;
					let timeString = startTime.split(":");
					let eHour = parseInt(timeString[0]);
					let eMin = parseInt(timeString[1]);
					let item=this.positionArray[this.itemIndex]
					if (item < 30) {
						eHour += item;
						if (eHour >= 24) {
							eHour -= 24;
					
					
						}
					} else {
						eMin = eMin + 30;
						if (eMin >= 60) {
							eHour += 1;
							eMin = eMin- 60;
					
						}
					}
					eHour = eHour < 10 ? '0' + eHour : eHour;
					eMin =eMin < 10 ? '0' + eMin : eMin;
					let endTime = `${eHour}:${eMin}:00`
					
					console.log(startTime);
					console.log(endTime);
					uni.request({
							url: `http://192.168.1.239:9900/app/office/now/use`,
						// url: 'http://82.157.34.130:9901/app/office/now/use',
						method:'POST',
						header:{
							'Content-Type': 'application/json',
							'Authorization': getApp().globalData.token
						},
						data: {
							'end_time':endTime,
							'start_time':startTime,
							'station_id':parseInt(that.id),
							'floor_id':1,
							'reserve_date':now,
						},
						success: (res) => {
							console.log(res)
                                     setTimeout(()=>{
										 uni.navigateTo({
										 	url: `../login-success/login-success?index=1&buttonIndex=1`
										 })
									 },500)
					



						}
					})
				}
			}



		},
		components: {
			tarbarHeader,
			tip,

		},


	}
</script>

<style>
	.bgc {

		height: 100vh;
		background-color: rgba(241, 242, 246, 1);

	}

	.head {
		height: calc(100vh *44/812);
		display: flex;
		flex-direction: row;

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

		width: calc(750rpx * 343/ 375);
		height: calc(100vh * 108/812);
		color: rgba(255, 255, 255, 1);
		margin-left: calc(750rpx * 16/ 375);
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		font-size: calc(750rpx * 14/ 375);
	}

	.content .top .top-item {
		width: calc(750rpx * 343/ 375);
		height: calc(100vh * 83/812);

		border-radius: calc(750rpx *30/ 375);
		box-sizing: border-box;
		background-color: rgba(10, 32, 57, 0.1);
		margin-top: calc(100vh * 24/812);



	}

	.content .center {


		width: calc(750rpx * 343/ 375);
		height: calc(100vh * 440/812);
		margin-top: calc(100vh * 20/812);
		margin-left: calc(750rpx * 16/ 375);

	}

	.content .center .center-head {
		width: calc(750rpx * 343/ 375);
		height: calc(100vh * 62/812);
		border-top-right-radius: calc(750rpx * 30/375);
		border-top-left-radius: calc(750rpx * 30/ 375);
		display: flex;
		align-items: center;
		background-color: rgba(248, 253, 253, 1);


	}

	.content .center .center-head text {
		color: #13C2C2;
		font-weight: bold;
		margin-left: 10%;
	}

	.content .center .center-boxOne {}

	.content .center .center-boxOne .center-boxOne-top {
		background-color: rgba(248, 253, 253, 1);
		border-bottom-right-radius: calc(750rpx * 30/375);
		border-bottom-left-radius: calc(750rpx * 30/ 375);
	}

	.content .center .center-boxOne .center-boxOne-top .center-boxOne-top-content {
		overflow: hidden;
		width: calc(750rpx * 295/ 375);
		height: calc(100vh * 232/812);
		margin-left: calc(750rpx * 24/ 375);
		border-top: dashed calc(100vh * 1/812) gray;
		display: flex;
		flex-wrap: wrap;
		align-content: flex-start;


	}

	.content .center .center-boxOne .center-boxOne-top .center-boxOne-top-content .center-boxOne-top-content-item {
		width: calc(750rpx * 137.5/ 375);
		height: calc(100vh * 40/812);
		line-height: calc(100vh * 40/812);
		text-align: center;
		border-radius: calc(750rpx * 10/ 375);
		border: rgba(17, 30, 54, 0.15) calc(750rpx * 1/ 375) solid;
		margin-top: calc(100vh * 24/812);
		box-sizing: border-box;




	}

	.content .center .center-boxOne .center-boxOne-top .center-boxOne-top-content .center-boxOne-top-content-item:nth-child(-n+2) {
		margin-top: calc(100vh * 32/812);

	}

	.content .center .center-boxOne .center-boxOne-top .center-boxOne-top-content .center-boxOne-top-content-item:nth-child(2n) {
		margin-left: calc(750rpx * 20/ 375);
	}

	.content .center .center-boxOne .center-boxOne-top .center-boxOne-top-content .center-boxOne-top-content-item.click {
		border-color: rgba(251, 178, 88, 1);
		color: rgba(251, 178, 88, 1);
	}


	.content .center .center-boxOne .center-boxOne-bottom {
		width: calc(750rpx * 343/ 375);
		height: calc(100vh * 76/812);
		margin-top: calc(100vh * 36/812);

	}

	.content .center .center-boxOne .center-boxOne-bottom .tip {
		margin-left: -5%;
	}

	.content .center .center-boxOne .center-boxOne-bottom .center-boxOne-bottom-centent {
		display: flex;
		width: calc(750rpx * 343/ 375);
		height: calc(100vh * 32/812);
		line-height: calc(100vh * 32/812);
		margin-top: calc(100vh * 14/812);
		border-radius: calc(750rpx * 10/ 375);
		background-color: white;
		position: relative;
	}

	.content .center .center-boxOne .center-boxOne-bottom .center-boxOne-bottom-centent::before {
		content: '~';
		position: absolute;

	}




	.content .bottom {
		width: calc(750rpx * 375/ 375);
		height: calc(100vh * 134/812);
		margin-top: calc(100vh * 16/812);
		overflow: hidden;

	}

	.content .bottom .resever {
		box-sizing: border-box;
		margin-top: calc(100vh * 11/812);
		margin-left: calc(750rpx * 16/ 375);
		margin-right: calc(750rpx * 16/ 375);
		width: calc(750rpx * 343/ 375);
		height: calc(100vh * 44/812);
		background-image: linear-gradient(135deg, #70CFBA 0%, #19BDC0 100%);
		border-radius: calc(750rpx * 12/ 375);
		font-size: calc(750rpx * 15/ 375);
		line-height: calc(100vh * 44/812);
		text-align: center;
		margin-bottom: ;
		color: white;
	}

	.content .bottom .cancal {
		box-sizing: border-box;
		margin-top: calc(100vh * 11/812);
		margin-left: calc(750rpx * 16/ 375);
		margin-right: calc(750rpx * 16/ 375);
		width: calc(750rpx * 343/ 375);
		height: calc(100vh * 44/812);
		background-color: white;
		border-radius: calc(750rpx * 12/ 375);
		font-size: calc(750rpx * 15/ 375);
		line-height: calc(100vh * 44/812);
		text-align: center;
		margin-bottom: ;
		color: rgba(19, 194, 194, 1);
		border: 1px solid rgba(19, 194, 194, 1);
	}
</style>
