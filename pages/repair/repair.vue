<template>
	<view class="bgc">
		<view class="content">
			<tarbarHeader class="head">
				<image slot='left' style="width:calc(750rpx * 26.42/ 375);height:calc(750rpx * 28.47	/ 375);"
					src="../../static/app/back.svg" @click="backClick"></image>
				<text slot='center'>设备报修</text>
				<text slot='right'></text>
			</tarbarHeader>
			<view class="main">
				<view class="main-top">
					<tip :item="{name:'设备报修'}">
						<text slot="right"></text>
					</tip>
				</view>
				<view class="main-center">

					<view class="main-center-item">
						<view class="main-center-item-left">
							故障原因：
						</view>
						<view class="main-center-item-right">
							<input placeholder="请输入" />
						</view>

					</view>
					<view class="main-center-item">
						<view class="main-center-item-left">
							故障说明：
						</view>
						<view class="main-center-item-right">
							<picker style='display: inline-block;'mode="selector" :range='array' :index="0" @change="bindPickerChange">
								<view style="margin-left: calc(750rpx * 150/ 375);margin-right:calc(750rpx * 8/ 375) ;">
									{{array[index]}}
								</view>
							</picker>
							<image src="../../static/app/icon-xiaoxi@2X.png" style="width: calc(750rpx * 7.51/ 375);height: calc(100vh *13.52/812);" ></image>
						</view>

					</view>
					<view class="main-center-item">
						<view class="main-center-item-left">
							故障原因：
						</view>
						<view class="main-center-item-right" >
							<textarea placeholder="请输入故障说明..." maxlength="300" confirm-type="0/300" style="font-size:calc(750rpx * 15/ 375); width: 100%;"/>
						</view>
					</view>
				</view>
				<view class="main-bottom">
					<button type="default">报修</button>
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
				array: ['插座没电', '插做损坏', '插座漏电'],
				index:0,
			}
		},
		onLoad() {

		},
		methods: {
			backClick() {
			uni.navigateBack();
			},
			   bindPickerChange: function(e) {
				        console.log(e.target.value)
			            console.log('picker发送选择改变，携带值为', e.target.value)
			            this.index = e.target.value
			        },
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
		height: calc(100vh * 424/812);
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
		height: calc(100vh *260/812);

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
		height: calc(100vh *160/812);
		display: flex;
		flex-direction: column;

		border-bottom: 1px dashed #E6E7E8;
	}

	.content .main .main-center .main-center-item .main-center-item-right {
		flex-grow: 1;
		font-size: calc(750rpx * 15/ 375);


	}

	.content .main .main-center .main-center-item .main-center-item-right input {
		font-size: calc(750rpx * 15/ 375);

	}

	.content .main .main-center .main-center-item:nth-child(3) .main-center-item-left {
		width: 100%;


	}

	.content .main .main-center .main-center-item:nth-child(3) .main-center-item-right {
		width: 100%;

	}

	.content .main .main-bottom {
		height: calc(100vh *105/812);

	}

	.content .main .main-bottom button {
		font-size:calc(750rpx * 15/ 375);
		width: calc(750rpx * 311/ 375);
		height: calc(100vh *44/812);
		border-radius: calc(750rpx * 8/ 375);
		margin-top: calc(100vh *31/812);
		background-image: linear-gradient(135deg, rgba(112, 207, 186, 1), rgba(25, 189, 192, 1));
		color: rgba(255, 255, 255, 1);
		line-height: calc(100vh *44/812);
	}
</style>
