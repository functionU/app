<template>
	<view class="bgc">
		<view class="content">
			<tarbarHeader class="head">
				<image slot='left' style="width:calc(750rpx * 26.42/ 375);height:calc(750rpx * 28.47/ 375);"
					src="../../static/app/back.svg" @click="backClick"></image>
				<text slot='center'></text>
				<text slot='right'
					style="font-size:calc(750rpx * 17/ 375) ; color: white;margin-left: calc(-750rpx * 220/ 375);">选择工位</text>
			</tarbarHeader>
			<view class="top">
				<view class="top-item">
					<view style="color: rgba(17, 30, 54, 1);">
						{{date}}
					</view>
					<text style="color: rgba(10, 32, 57, 0.3); ">|</text>
					<view style="color: #007AFF;">
						{{startTime}}--{{endTime}}
					</view>
				</view>
				<view class="top-item">
					<view>
						<view>
							<image src="../../static/app/info@2X.png"
								style="width: calc(750rpx * 10.64/ 375);height: calc(750rpx * 14/ 375); transform: translateY(10%);">
							</image>
						</view>
						<view
							style="width: calc(750rpx * 84/ 375);text-overflow: ellipsis; white-space:nowrap;overflow:hidden;">
							{{place}}
						</view>

					</view>
					<view style="margin-left: calc(750rpx * 8/ 375);">
						<view>
							{{floor}}F
						</view>
						<view>
							<image src="../../static/app/info@2X.png"
								style="width: calc(750rpx * 14/ 375);height: calc(750rpx * 14/ 375); transform: translateY(10%);">
							</image>
						</view>

					</view>
				</view>
			</view>
			<view class="center">
				<view class="center-head">
					<view class="list" @click="listClick" :class="{'click':show}">
						<image
							style="width:calc(750rpx * 13.98/ 375);height:calc(750rpx * 11.65/ 375);margin-right:calc(750rpx * 8.02/ 375) ;margin-left: calc(750rpx * 26/ 375) ;"
							src="../../static/app/icon-zhishu@2X.png"></image>
						<text style="margin-right:calc(750rpx * 36/ 375) ;">列表</text>
					</view>
					<view class="map" @click="mapClick" :class="{'click':!show}">
						<image
							style="width:calc(750rpx * 13.98/ 375);height:calc(750rpx * 11.65/ 375);margin-right:calc(750rpx * 8.02/ 375)"
							src="../../static/app/icon-zhishu@2X.png"></image>
						<text style="margin-right:calc(750rpx * 36/ 375) ;">地图</text>
					</view>
				</view>
				<view class="center-boxOne" v-show="show">
					<view class="center-boxOne-top">
						<view class="center-boxOne-top-content">
							<view @click="itemClick(item,index)" :class="{'click':index==itemIndex}"
								class="center-boxOne-top-content-item" v-for="(item,index) in positionArray">
								{{item}}
							</view>
						</view>
					</view>
					<view class="center-boxOne-bottom">
						<tip :item="{name:'常用工位 '}">
							<text slot="right" style="margin-left:calc(-750rpx * 200/ 375);">(推荐)</text>
						</tip>
						<view class="center-boxOne-bottom-centent">
							<view @click="usuallyItemClick(item,index)" :class="{'click':index==usuallyItemIndex}"
								class="center-boxOne-bottom-centent-item" v-for="(item,index) in UsuallyArray">
								{{item}}
							</view>

						</view>
					</view>
				</view>
				<view class="center-boxTwo" v-show="!show">

				</view>
			</view>
			<view class="bottom">
				<view @click="reseverFinshed">
					立即预订（{{itemName}}）
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
				usuallyItemIndex: -1,
				itemIndex: -1,
				itemName: '',
				startTime: "",
				date: "",
				endTime: "",
				plcae: "",
				floor: "",
				name: "",
				show: true,
			}

		},
		props: {
			positionArray: {
				type: Array,
				default: () => {
					return ['YJ051', 'YJ051', 'YJ051', 'YJ051', 'YJ051', 'YJ051', 'YJ051', 'YJ051', 'YJ051', 'YJ051',
						'YJ051', 'YJ051', 'YJ051', 'YJ051', 'YJ051', 'YJ051', 'YJ051', 'YJ051', 'YJ051', 'YJ052',
						'YJ051', 'YJ051', 'YJ051', 'YJ051', 'YJ051', 'YJ051', 'YJ051', 'YJ051', 'YJ051', 'YJ051',
						'YJ051', 'YJ051', 'YJ051', 'YJ051', 'YJ051', 'YJ052'
					]
				}
			},
			UsuallyArray: {
				type: Array,
				default: () => {
					return ['YJ051', 'YJ051']
				}
			}
		},
		onLoad(option) {

			this.startTime = option.startTime;
			this.endTime = option.endTime;
			this.date = option.date;
			this.place = option.place;
			this.floor = option.floor;

		},

		methods: {

			backClick() {
				uni.navigateBack({

				})
			},
			itemClick(item, index) {
				this.itemName = item
				this.itemIndex = index;
			},
			usuallyItemClick(item, index) {

				this.itemName = item
				this.usuallyItemIndex = index;
			},
			mapClick() {
				this.show = false;
			},
			listClick() {
				this.show = true;
			},
			reseverFinshed() {

				if (!this.itemName) {
					uni.showModal({
						title: '提示',
						content: '请选择工位',
						showCancel: false,
					});
				} else {
					let obj = {};

					obj.startTime = this.startTime;
					obj.endTime = this.endTime;
					obj.date = this.date;
					obj.place = this.place;
					obj.floor = this.floor;
					obj.position = this.itemName;

					uni.navigateTo({
						url: `../login-success/login-success?resever=true&index=1&buttonIndex=1`
					});
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

	.content .top .top-item:nth-child(1) {
		width: calc(750rpx * 343/ 375);
		height: calc(100vh * 50/812);
		border-radius: calc(750rpx * 12/ 375);
		background-color: rgba(255, 255, 255, 1);
		display: flex;
		justify-content: space-around;
		align-items: center;

	}

	.content .top .top-item:nth-child(2) {
		width: calc(750rpx * 343/ 375);
		height: calc(100vh * 32/812);

		font-size: calc(750rpx * 12/ 375);
		display: flex;
	}

	.content .top .top-item:nth-child(2)>view:nth-child(1) {
		width: calc(750rpx * 134/ 375);
		line-height: calc(100vh * 32/812);
		border-radius: calc(750rpx * 12/ 375);
		background-color: rgba(10, 32, 57, 0.1);
		display: flex;
		justify-content: space-around;
		align-items: center;


	}

	.content .top .top-item:nth-child(2)>view:nth-child(2) {
		width: calc(750rpx * 92/ 375);
		line-height: calc(100vh * 32/812);
		border-radius: calc(750rpx * 12/ 375);
		background-color: rgba(10, 32, 57, 0.1);
		display: flex;
		justify-content: space-around;
		align-items: center;

	}

	.content .center {

		width: calc(750rpx * 343/ 375);
		height: calc(100vh * 484/812);
		margin-top: calc(100vh * 32/812);
		margin-left: calc(750rpx * 16/ 375);

	}

	.content .center .center-head {
		width: calc(750rpx * 343/ 375);
		height: calc(100vh * 61/812);
		border-top-right-radius: calc(750rpx * 30/375);
		border-top-left-radius: calc(750rpx * 30/ 375);
		display: flex;
		align-items: center;
		background-color: rgba(248, 253, 253, 1);


	}

	.content .center .center-head .map {
		position: relative;

	}

	.content .center .center-head .list {

		position: relative;

	}

	.content .center .center-head .click {
		color: #13C2C2;
		font-weight: bold;
	}

	.content .center .center-head .list.click::before {
		content: '';
		display: block;
		width: calc(750rpx * 6/ 375);
		height: calc(750rpx * 6/ 375);
		border-radius: 50%;
		background-color: #1ABFC2;
		position: absolute;
		left: 50%;
		top: 120%;
	}

	.content .center .center-head .map.click::before {
		content: '';
		display: block;
		width: calc(750rpx * 6/ 375);
		height: calc(750rpx * 6/ 375);
		border-radius: 50%;
		background-color: #1ABFC2;
		position: absolute;
		left: 35%;
		top: 120%;
	}

	.content .center .center-boxOne {}

	.content .center .center-boxOne .center-boxOne-top {
		background-color: rgba(248, 253, 253, 1);
		border-bottom-right-radius: calc(750rpx * 30/375);
		border-bottom-left-radius: calc(750rpx * 30/ 375);
	}

	.content .center .center-boxOne .center-boxOne-top .center-boxOne-top-content {
		overflow: scroll;
		width: calc(750rpx * 295/ 375);
		height: calc(100vh * 312/812);

		margin-left: calc(750rpx * 26/ 375);
		border-top: dashed calc(100vh * 1/812) gray;
		display: flex;
		flex-wrap: wrap;
		align-items: center;
	}

	.content .center .center-boxOne .center-boxOne-top .center-boxOne-top-content .center-boxOne-top-content-item {
		width: calc(750rpx * 93/ 375);
		height: calc(100vh * 40/812);
		line-height: calc(100vh * 40/812);
		text-align: center;
		border-radius: calc(750rpx * 10/ 375);
		box-sizing: border-box;
		border: rgba(17, 30, 54, 0.15) calc(750rpx * 1/ 375) solid;
		margin-top: calc(100vh * 24/812);
		margin-left: calc(750rpx * 8/ 375);
	}

	.content .center .center-boxOne .center-boxOne-top .center-boxOne-top-content .center-boxOne-top-content-item.click {
		border-color: rgba(251, 178, 88, 1);

		color: rgba(251, 178, 88, 1);
	}

	.content .center .center-boxOne .center-boxOne-top .center-boxOne-top-content .center-boxOne-top-content-item:nth-child(3n-2) {
		margin-left: 0;

	}

	.content .center .center-boxOne .center-boxOne-bottom {
		width: calc(750rpx * 343/ 375);
		height: calc(100vh * 76/812);


	}

	.content .center .center-boxOne .center-boxOne-bottom .center-boxOne-bottom-centent {
		display: flex;

	}

	.content .center .center-boxOne .center-boxOne-bottom .center-boxOne-bottom-centent .center-boxOne-bottom-centent-item {
		width: calc(750rpx * 93/ 375);
		height: calc(100vh * 40/812);
		line-height: calc(100vh * 40/812);
		text-align: center;
		border-radius: calc(750rpx * 10/ 375);
		border: rgba(17, 30, 54, 0.15) calc(750rpx * 1/ 375) solid;
		margin-top: calc(100vh * 24/812);
		margin-left: calc(750rpx * 8/ 375);
		position: relative;
	}

	.content .center .center-boxOne .center-boxOne-bottom .center-boxOne-bottom-centent .center-boxOne-bottom-centent-item:first-child::before {
		content: '';
		display: block;
		width: calc(750rpx * 8/ 375);
		height: calc(750rpx * 8/ 375);
		border-radius: 50%;
		background-color: #1ABFC2;
		position: absolute;
		left: 80%;
		top: 40%;
	}

	.content .center .center-boxOne .center-boxOne-bottom .center-boxOne-bottom-centent .center-boxOne-bottom-centent-item.click {
		border-color: rgba(251, 178, 88, 1);
		color: rgba(251, 178, 88, 1);
	}

	.content .center .center-boxTwo {
		width: calc(750rpx * 343/ 375);
		height: calc(100vh * 423/812);
		border-bottom-right-radius: calc(750rpx * 30/375);
		border-bottom-left-radius: calc(750rpx * 30/ 375);
		background-color: #007AFF;
	}

	.content .bottom {
		width: calc(750rpx * 375/ 375);
		height: calc(100vh * 90/812);
		margin-top: calc(100vh * 16/812);
		background-color: white;

		overflow: hidden;
	}

	.content .bottom view {

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
</style>
