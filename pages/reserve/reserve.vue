<template>
	<view class="bgc">
		<view class="content">
			<tarbarHeader class="head">
				<image slot='left' style="width:calc(750rpx * 26.42/ 375);height:calc(750rpx * 28.47/ 375);"
					src="../../static/app/icon-jiankang@2X.png" @click="backClick"></image>
				<text slot='center' >筛选</text>
				<text slot='right'></text>
			</tarbarHeader>
			<view class="main">
				<view class="main-top">
					<tip :item="{name:'选择时间'}">
						<text slot="right"></text>
					</tip>
				</view>
				<view class="main-center">


					<view class="main-center-item">
						<view class="main-center-item-left">
							选择日期：
						</view>
						<view class="main-center-item-right">
							<picker style='display: inline-block;' mode="date" start="startDate" end="endDate"
								@change="bindDatePickerChange">
								<view>
									{{date}}
								</view>
							</picker>
							<image src="../../static/app/icon-xiaoxi@2X.png"
								style="width: calc(750rpx * 7.51/ 375);height: calc(100vh *13.52/812);"></image>
						</view>

					</view>
					<view class="main-center-item">
						<view class="main-center-item-left">
							开始时间：
						</view>
						<view class="main-center-item-right">
							<picker style='display: inline-block;' mode="time" start="startTime" end="23:59"
								:value="startTime" @change="bindStartTimePickerChange">
								<view>
									{{startTime}}
								</view>
							</picker>
							<image src="../../static/app/icon-xiaoxi@2X.png"
								style="width: calc(750rpx * 7.51/ 375);height: calc(100vh *13.52/812);"></image>
						</view>

					</view>
					<view class="main-center-item">
						<view class="main-center-item-left">
							结束时间：
						</view>
						<view class="main-center-item-right">
							<picker style='display: inline-block;' mode="time" start="00:00" end="23:59"
								:value="startTime" @change="bindEndTimePickerChange">
								<view>
									{{endTime}}
								</view>
							</picker>
							<image src="../../static/app/icon-xiaoxi@2X.png"
								style="width: calc(750rpx * 7.51/ 375);height: calc(100vh *13.52/812);"></image>
						</view>

					</view>
				</view>
				<view class="main-top">
					<tip :item="{name:'选择位置'}">
						<text slot="right"></text>
					</tip>
				</view>
				<view class="main-center">


					<view class="main-center-item">
						<view class="main-center-item-left">
							楼宇：
						</view>
						<view class="main-center-item-right">
							<picker style='display: inline-block;' mode="selector" :range='placeArray'
								:index="placeIndex" @change="bindPlacePickerChange">
								<view>
									{{placeArray[placeIndex]}}
								</view>
							</picker>
							<image src="../../static/app/icon-xiaoxi@2X.png"
								style="width: calc(750rpx * 7.51/ 375);height: calc(100vh *13.52/812);"></image>
						</view>

					</view>
					<view class="main-center-item">
						<view class="main-center-item-left">
							楼层：
						</view>
						<view class="main-center-item-right">
							<picker style='display: inline-block;' mode="selector" :range='floorArray'
								:index="floorIndex" @change="bindFloorPickerChange">
								<view>
									{{floorArray[floorIndex]}}
								</view>
							</picker>
							<image src="../../static/app/icon-xiaoxi@2X.png"
								style="width: calc(750rpx * 7.51/ 375);height: calc(100vh *13.52/812);"></image>
						</view>

					</view>

				</view>
				<view class="main-bottom">
					<button type="default" @click="reservePosition">下一步（选择工位）</button>
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
			const currentDate = this.getDate();
			const hours = this.getDate('time')
			return {
				placeArray: ['中国', '美国', '俄罗斯'],
				placeIndex: 0,
				floorArray: ['1', '2', '3', '4', '5'],
				floorIndex: 0,
				date: currentDate,
				startTime: hours,
				endTime: hours,
			}
		},
		onLoad() {

		},
		methods: {
			getDate(type) {
				const date = new Date();
				let year = date.getFullYear();
				let month = date.getMonth() + 1;
				let day = date.getDate();
				let hours = date.getHours();
				let mins = date.getMinutes()
				if (type === 'start') {
					year = year - 60;
				} else if (type === 'end') {
					year = year + 2;
				}
				hours = hours > 9 ? hours : '0' + hours;
				mins = mins > 9 ? mins : '0' + mins;
				month = month > 9 ? month : '0' + month;
				day = day > 9 ? day : '0' + day;
				if (type == 'time') {
					return `${hours}:${mins}`;
				}
				return `${year}-${month}-${day}`;
			},
			backClick() {
				uni.navigateBack();
			},
			bindDatePickerChange(e) {
				this.date = e.target.value
			},
			bindStartTimePickerChange(e) {
				this.startTime = e.target.value
			},
			bindEndTimePickerChange(e) {
				this.endTime = e.target.value
			},
			bindPlacePickerChange(e) {
				this.placeIndex = e.target.value
			},
			bindFloorPickerChange(e) {
				this.floorIndex = e.target.value
			},
			reservePosition() {
				let end = this.endTime.split(":");
				let start = this.startTime.split(":");

				if ((end[0] - start[0]) < 0 || ((end[0] - start[0]) == 0 && (end[1] <= start[1]))) {
					uni.showModal({
						title: '提示',
						content: '结束时间需大于开始时间',
						showCancel: false,
					});
				} else
					uni.navigateTo({
						url: `./reservePosition?date=${this.date}&startTime=${this.startTime}&endTime=${this.endTime}&place=${this.placeArray[this.placeIndex]}&floor=${this.floorArray[this.floorIndex]}`
					})
			}
		},
		components: {
			tarbarHeader,
			tip,

		},
		computed: {
			startDate() {
				return this.getDate('start');
			},
			endDate() {
				return this.getDate('end');
			}
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
		height: calc(100vh * 474/812);
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

	}

	.content .main .main-center .main-center-item {
		display: flex;
		justify-content: space-between;

	}

	.content .main .main-center .main-center-item .main-center-item-right image {
		margin-left: calc(750rpx * 8 / 375);
	}

	.content .main .main-bottom {
		height: calc(100vh *105/812);

	}

	.content .main .main-bottom button {
		font-size: calc(750rpx * 15/ 375);
		width: calc(750rpx * 311/ 375);
		height: calc(100vh *44/812);
		border-radius: calc(750rpx * 8/ 375);
		margin-top: calc(100vh *11/812);
		background-image: linear-gradient(135deg, rgba(112, 207, 186, 1), rgba(25, 189, 192, 1));
		color: rgba(255, 255, 255, 1);
		line-height: calc(100vh *44/812);
	}
</style>
