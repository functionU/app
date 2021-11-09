<template>
	<view class="bgc">
		<view class="content">
			<tarbarHeader class="head">
				<image slot='left' style="width:calc(750rpx * 26.42/ 375);height:calc(750rpx * 28.47/ 375);"
					src="../../static/app/icon-jiankang@2X.png" @click="backClick"></image>
				<text slot='center'>筛选</text>
				<text slot='right'></text>
			</tarbarHeader>
			<view class="main">
				<view class="main-top">
					<tip :item="{name:'选择时间'}">
						<text slot="right"></text>
					</tip>
				</view>
				<view class="main-center">


					<view class="main-center-item" @click="dateClick">
						<view class="main-center-item-left">
							选择日期：
						</view>
						<view class="main-center-item-right">

							<view style="display: inline-block;">
								{{year}}-{{month}}-{{day}}
							</view>

							<image src="../../static/app/icon-xiaoxi@2X.png"
								style="width: calc(750rpx * 7.51/ 375);height: calc(100vh *13.52/812);"></image>
						</view>

					</view>
					<view class="main-center-item" @click="startTimeClick">
						<view class="main-center-item-left">
							开始时间：
						</view>
						<view class="main-center-item-right">

							<view style="display:inline-block;">
								{{startHour}}:{{startMin}}
							</view>

							<image src="../../static/app/icon-xiaoxi@2X.png"
								style="width: calc(750rpx * 7.51/ 375);height: calc(100vh *13.52/812);"></image>
						</view>

					</view>
					<view class="main-center-item" @click="endTimeClick">
						<view class="main-center-item-left">
							结束时间：
						</view>
						<view class="main-center-item-right">

							<view style="display: inline-block;">
								{{endHour}}:{{endMin}}
							</view>

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


					<view class="main-center-item"  @click="placeClick">
						<view class="main-center-item-left">
							楼宇：
						</view>
						<view class="main-center-item-right">

							<view style="display: inline-block;">
								{{placeArray[placeIndex[0]]}}
							</view>

							<image src="../../static/app/icon-xiaoxi@2X.png"
								style="width: calc(750rpx * 7.51/ 375);height: calc(100vh *13.52/812);"></image>
						</view>

					</view>
					<view class="main-center-item" @click="floorClick">
						<view class="main-center-item-left">
							楼层：
						</view>
						<view class="main-center-item-right">

							<view style="display: inline;">
								{{floorArray[floorIndex[0]]}}
							</view>

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
		<view class="pickerDate" style="height: 100vh;" v-show="false">
			<view style="display:flex;justify-content: space-between;background-color: white; ">
				<text style="font-size: calc(750rpx * 14/ 375);padding: calc(750rpx * 15/ 375);">取消</text>
				<text
					style="font-size: calc(750rpx * 14/ 375);padding: calc(750rpx * 15 375) ;color: rgba(19, 194, 194, 1);">确定</text>
			</view>
			<picker-view style="background-color: white; height: 400px;text-align: center;"
				@change="bindDatePickerChange" :value="valueDate">
				<picker-view-column>
					<view class="item" v-for="(item,index) in years">{{item}}年</view>
				</picker-view-column>
				<picker-view-column>
					<view class="item" v-for="(item,index) in months">{{item}}月</view>
				</picker-view-column>
				<picker-view-column>
					<view class="item" v-for="(item,index) in days">{{item}}日</view>
				</picker-view-column>
			</picker-view>
		</view>
		<view class="pickerStartTime" style="height: 100vh;" v-show="false">
			<view style="display:flex;justify-content: space-between;background-color: white; ">
				<text style="font-size: calc(750rpx * 14/ 375);padding: calc(750rpx * 9/ 375);">取消</text>
				<text
					style="font-size: calc(750rpx * 14/ 375);padding: calc(750rpx * 9 /375) ;color: rgba(19, 194, 194, 1);">确定</text>
			</view>
			<picker-view style="background-color: white; height: 400px;text-align: center;"
				@change="bindStartTimePickerChange" :value="valueStartTime">
				<picker-view-column>
					<view class="item" v-for="(item,index) in hours">{{item}}点</view>
				</picker-view-column>
				<picker-view-column>
					<view class="item" v-for="(item,index) in mins">{{item}}分</view>
				</picker-view-column>

			</picker-view>
		</view>
		<view class="pickerStartTime" style="height: 100vh;" v-show="false">
			<view style="display:flex;justify-content: space-between;background-color: white; ">
				<text style="font-size: calc(750rpx * 14/ 375);padding: calc(750rpx * 9/ 375);">取消</text>
				<text
					style="font-size: calc(750rpx * 14/ 375);padding: calc(750rpx * 9 /375) ;color: rgba(19, 194, 194, 1);">确定</text>
			</view>
			<picker-view style="background-color: white; height: 400px;text-align: center;"
				@change="bindEndTimePickerChange" :value="valueStartTime">
				<picker-view-column>
					<view class="item" v-for="(item,index) in hours">{{item}}点</view>
				</picker-view-column>
				<picker-view-column>
					<view class="item" v-for="(item,index) in mins">{{item}}分</view>
				</picker-view-column>

			</picker-view>
		</view>
		<view class="pickerEndTime" style="height: 100vh;" v-show="false">
			<view style="display:flex;justify-content: space-between;background-color: white; ">
				<text style="font-size: calc(750rpx * 14/ 375);padding: calc(750rpx * 9/ 375);">取消</text>
				<text
					style="font-size: calc(750rpx * 14/ 375);padding: calc(750rpx * 9 /375) ;color: rgba(19, 194, 194, 1);">确定</text>
			</view>
			<picker-view style="background-color: white; height: 400px;text-align: center;"
				@change="bindPlacePickerChange" :value="placeIndex">
				<picker-view-column>
					<view class="item" v-for="(item,index) in placeArray">{{item}}</view>
				</picker-view-column>

			</picker-view>
		</view>
		<view class="pickerEndTime" style="height: 100vh;">
			<view style="display:flex;justify-content: space-between;background-color: white; ">
				<text style="font-size: calc(750rpx * 14/ 375);padding: calc(750rpx * 9/ 375);">取消</text>
				<text
					style="font-size: calc(750rpx * 14/ 375);padding: calc(750rpx * 9 /375) ;color: rgba(19, 194, 194, 1);">确定</text>
			</view>
			<picker-view style="background-color: white; height: 400px;text-align: center;"
				@change="bindFloorPickerChange" :value="floorIndex">
				<picker-view-column>
					<view class="item" v-for="(item,index) in floorArray">{{item}}</view>
				</picker-view-column>
			</picker-view>
		</view>

	</view>


</template>\
<script>
	import tarbarHeader from '../../components/common/header/header.vue'
	import tip from '../../components/common/tip/tip.vue'


	export default {

		data() {
			const date = new Date();
			const years = [];
			const months = [];
			const days = [];
			const hours = [];
			const mins = [];
			let year = date.getFullYear();
			let month = date.getMonth() + 1;
			let day = date.getDate();
			let hour = date.getHours();
			let min = date.getMinutes();


			for (let i = 1990; i <= date.getFullYear(); i++) {
				years.push(i)
			}
			for (let i = 1; i <= 12; i++) {
				months.push(i)
			}
			for (let i = 1; i <= 31; i++) {
				days.push(i)
			}
			for (let i = 0; i <= 23; i++) {

				if (i < 10) {
					hours.push("0" + i);
				} else
					hours.push(i);
			}
			for (let i = 0; i <= 59; i++) {

				if (i < 10) {
					mins.push("0" + i);
				} else
					mins.push(i)
			}

			return {
				placeArray: ['中国', '美国', '俄罗斯'],
				placeIndex: [0],
				floorArray: ['1', '2', '3', '4', '5'],
				floorIndex: [0],
				years,
				months,
				days,
				year,
				month,
				day,
				hours,
				mins,
				startHour: hour,
				startMin: min,
				endHour: hour,
				endMin: min,
				valueDate: [9999, month - 1, day - 1],
				valueStartTime: [hour, min],

			}
		},
		onLoad() {

		},
		methods: {

			backClick() {
				uni.navigateBack();
			},
			bindDatePickerChange(e) {
				let val = e.target.value;
				this.year = this.years[val[0]];
				this.month = this.months[val[1]];
				this.day = this.days[val[2]];
			},
			bindStartTimePickerChange(e) {
				let val = e.target.value;
				this.startHour = this.hours[val[0]];
				this.startMin = this.mins[val[1]];
			},
			bindEndTimePickerChange(e) {
				let val = e.target.value;
				this.startHour = this.hours[val[0]];
				this.startMin = this.mins[val[1]];
			},
			bindPlacePickerChange(e) {
				let val = e.target.value;
				this.placeIndex[0] = val[0]
			},
			bindFloorPickerChange(e) {
				let val = e.target.value;
				this.floorIndex[0] = val[0]
			},
			reservePosition() {

				if (1) {
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
