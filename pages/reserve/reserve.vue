<template>
	<view class="bgc">
		<view class="content">
			<tarbarHeader class="head">
				<image slot='left' style="width:calc(750rpx * 26.42/ 375);height:calc(750rpx * 28.47/ 375);"
					src="../../static/app/back.svg" @click="backClick"></image>
				<text slot='center'
					style="display: block;text-align: center;font-size:calc(750rpx * 17/ 375);color: #FFFFFF;">筛选</text>
				<text slot='right'></text>
			</tarbarHeader>
			<view class="main" >
				<view class="main-top">
					<tip :item="{name:'选择时间'}" >
						<text slot="right"></text>
					</tip>
				</view>
				<view class="main-center" 	>


					<view class="main-center-item" @click="dateClick">
						<view class="main-center-item-left">
							选择日期：
						</view>
						<view class="main-center-item-right">

							<view style="display: inline;">
								{{year}}-{{month}}-{{day}}
							</view>

							<image src="../../static/app/next.svg"
								style="width: calc(750rpx * 7.51/ 375);height: calc(100vh *13.52/812);"></image>
						</view>

					</view>
					<view class="main-center-item" @click="startTimeClick">
						<view class="main-center-item-left">
							开始时间：
						</view>
						<view class="main-center-item-right">

							<view style="display: inline;">
								{{startHour}}:{{startMin}}
							</view>

							<image src="../../static/app/next.svg"
								style="width: calc(750rpx * 7.51/ 375);height: calc(100vh *13.52/812);"></image>
						</view>

					</view>
					<view class="main-center-item" @click="endTimeClick">
						<view class="main-center-item-left">
							结束时间：
						</view>
						<view class="main-center-item-right">

							<view style="display: inline;">
								{{endHour}}:{{endMin}}
							</view>

							<image src="../../static/app/next.svg"
								style="width: calc(750rpx * 7.51/ 375);height: calc(100vh *13.52/812);"></image>
						</view>

					</view>
				</view>
				<view class="main-top">
					<tip :item="{name:'选择位置'}" >
						<text slot="right"></text>
					</tip>
				</view>
				<view class="main-center">


					<view class="main-center-item" @click="placeClick">
						<view class="main-center-item-left">
							楼宇：
						</view>
						<view class="main-center-item-right">

							<view style="display: inline;">
								{{placeArray[placeIndex].name}}
							</view>

							<image src="../../static/app/next.svg"
								style="width: calc(750rpx * 7.51/ 375);height: calc(100vh *13.52/812);"></image>
						</view>

					</view>
					<view class="main-center-item" @click="floorClick">
						<view class="main-center-item-left">
							楼层：
						</view>
						<view class="main-center-item-right">

							<view style="display: inline;" v-show="floorArray[floorIndex].name">
								{{floorArray[floorIndex].name}}
							</view>

							<image src="../../static/app/next.svg"
								style="width: calc(750rpx * 7.51/ 375);height: calc(100vh *13.52/812);"></image>
						</view>

					</view>

				</view>
				<view class="main-bottom" style="margin-top:calc(100vh * 30/812);">
					<button type="default" @click="reservePosition">下一步（选择工位）</button>
				</view>

			</view>
		</view>
		<view class="pickerDate">
			<view :class="{'show':dateindex,'hidden':!dateindex}">
				<view style="display:flex;justify-content: space-between;background-color:white;" @click="dateClick"
					class="viewText">
					<text style="font-size: calc(750rpx * 14/ 375);padding: calc(750rpx * 9/ 375);"
						@click.stop="cancel('date')">取消</text>
					<text
						style="font-size: calc(750rpx * 14/ 375);color: rgba(19, 194, 194, 1);padding: calc(750rpx * 9/ 375);"
						@click.stop="confirm('data')">确定</text>
				</view>
				<picker-view style="background-color: white; height: calc(100vh *260/812);text-align: center;"
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

		</view>
		<view class="pickerStartTime ">
			<view :class="{'show':startindex,'hidden':!startindex}">
				<view style="display:flex;justify-content: space-between;background-color: white; "
					@click="startTimeClick" class="viewText">
					<text style="font-size: calc(750rpx * 14/ 375);padding: calc(750rpx * 9/ 375);"
						@click.stop="cancel('start')">取消</text>
					<text
						style="font-size: calc(750rpx * 14/ 375);padding: calc(750rpx * 9 /375) ;color: rgba(19, 194, 194, 1);"
						@click.stop="confirm('start')">确定</text>
				</view>
				<picker-view style="background-color: white; height: calc(100vh *260/812);text-align: center;"
					@change="bindStartTimePickerChange" :value="valueStartTime">
					<picker-view-column>
						<view class="item" v-for="(item,index) in hours">{{item}}点</view>
					</picker-view-column>
					<picker-view-column>
						<view class="item" v-for="(item,index) in mins">{{item}}分</view>
					</picker-view-column>

				</picker-view>
			</view>

		</view>
		<view class="pickerEndTime ">
			<view :class="{'show':endindex,'hidden':!endindex}">
				<view style="display:flex;justify-content: space-between;background-color: white; "
					@click="endTimeClick" class="viewText">
					<text style="font-size: calc(750rpx * 14/ 375);padding: calc(750rpx * 9/ 375);"
						@click.stop="cancel('end')">取消</text>
					<text
						style="font-size: calc(750rpx * 14/ 375);padding: calc(750rpx * 9 /375) ;color: rgba(19, 194, 194, 1);"
						@click.stop="confirm('end')">确定</text>
				</view>
				<picker-view style="background-color: white;height: calc(100vh *260/812);text-align: center;"
					@change="bindEndTimePickerChange" :value="valueEndTime">
					<picker-view-column>
						<view class="item" v-for="(item,index) in hours">{{item}}点</view>
					</picker-view-column>
					<picker-view-column>
						<view class="item" v-for="(item,index) in mins">{{item}}分</view>
					</picker-view-column>

				</picker-view>
			</view>

		</view>
		<view class="pickerPlace ">
			<view :class="{'show':placeindex,'hidden':!placeindex}">
				<view style="display:flex;justify-content: space-between;background-color: white; " @click="placeClick"
					class="viewText">
					<text style="font-size: calc(750rpx * 14/ 375);padding: calc(750rpx * 9/ 375);"
						@click.stop="cancel('place')">取消</text>
					<text
						style="font-size: calc(750rpx * 14/ 375);padding: calc(750rpx * 9 /375) ;color: rgba(19, 194, 194, 1);"
						@click.stop="confirm('place')">确定</text>
				</view>
				<picker-view style="background-color: white;height: calc(100vh *260/812);text-align: center;"
					@change="bindPlacePickerChange" :value="valuePlace">
					<picker-view-column>
						<view class="item" v-for="(item,index) in placeArray">{{item.name}}</view>
					</picker-view-column>

				</picker-view>
			</view>

		</view>
		<view class="pickerFloor">
			<view :class="{'show':floorindex,'hidden':!floorindex}">
				<view style="display:flex;justify-content: space-between;background-color: white; " @click="floorClick"
					class="viewText">
					<text style="font-size: calc(750rpx * 14/ 375);padding: calc(750rpx * 9/ 375);"
						@click.stop="cancel('floor')">取消</text>
					<text
						style="font-size: calc(750rpx * 14/ 375);padding: calc(750rpx * 9 /375) ;color: rgba(19, 194, 194, 1);"
						@click.stop="confirm('floor')">确定</text>
				</view>
				<picker-view style="background-color: white;height: calc(100vh *260/812);text-align: center;"
					@change="bindFloorPickerChange" :value="valueFloor">
					<picker-view-column>
						<view class="item" v-for="(item,index) in floorArray">{{item.name}}</view>
					</picker-view-column>

				</picker-view>
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
			const years = [];
			const months = [];
			const days = [];
			const hours = [];
			const mins = [];
			let year = date.getFullYear();
			let month = date.getMonth() + 1;
			let day = date.getDate();
			let hour = function() {
				let hour = date.getHours();
				if (hour < 10) {
					return '0' + hour;
				}
				return hour
			}();
			let min = function() {
				let min = date.getMinutes();
				if (min < 10) {
					return '0' + min;
				}
				return min
			}();


			for (let i = date.getFullYear(); i <= date.getFullYear() + 10; i++) {
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
				placeArray: [{
					name: ''
				}],
				placeIndex: 0,
				floorArray: [{
					name: ""
				}],
				floorIndex: 0,
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
				endHour: hour+1,
				endMin: min,
				valueDate: [0, month - 1, day - 1],
				valuePlace: [0],
				valueFloor: [0],
				valueStartTime: [hour, min],
				valueEndTime: [hour+1, min],
				dateindex: false,
				startindex: false,
				endindex: false,
				placeindex: false,
				floorindex: false,
				endMiddel: [hour+1, min],
				startMiddel: [hour, min],
				dateMiddel: [0, month - 1, day - 1],
				placeMiddel: [0],
				floorMiddel: [0],

			}
		},
		onLoad() {

			if (!getApp().globalData.token) {
				uni.navigateTo({
					url: "../login/login"
				})
			}

			let that = this;
			uni.request({
				url: `http://${getApp().globalData.http}/app/office/building/list`,
				// url: 'http://82.157.34.130:9901/app/office/building/list',

				header: {
					'Content-Type': 'application/json',
					'Authorization': getApp().globalData.token,
				},

				success: (res) => {

					if (Array.isArray(res.data.value)) {
						let newArray = [];
						res.data.value.map((item) => {

							newArray.push(item);
						})
						that.placeArray = newArray;
						let id = this.placeArray[this.placeIndex].id;
						uni.request({
							url: `http://${getApp().globalData.http}/app/office/building/floor/list/${id}`,
							// url: `http://82.157.34.130:9901/app/office/building/floor/list/${id}`,

							header: {
								'Content-Type': 'application/json',
								'Authorization': getApp().globalData.token,
							},
							success: (res) => {


								if (res.data.code == 0 && res.data.value.length > 0) {
									let newArray = [];
									res.data.value.map((item) => {
										newArray.push(item);
									})
									that.floorArray = res.data.value;
									that.valueFloor = [0];

								} else {
									uni.showToast({
										title: '当前楼宇无楼层信息',
										icon: 'none',
										duration: 2000
									})

								}




							}
						})
					}





				}
			})

		},
		methods: {

			backClick() {
				uni.navigateBack();
			},

			reservePosition() {




				if ((this.endHour - this.startHour) < 0 || ((this.endHour == this.startHour) && (parseInt(this.endMin) <=
						parseInt(this.startMin)))) {
					uni.showModal({
						title: '提示',
						content: '结束时间需大于开始时间',
						showCancel: false,
					});
				} else {
					let date = `${this.year}-${this.month}-${this.day}`;
					let startTime = `${this.startHour}:${this.startMin}`;
					let endTime = `${this.endHour}:${this.endMin}`
					let that = this;

					uni.navigateTo({
						url: `./reservePosition?date=${date}&startTime=${startTime}&endTime=${endTime}&place=${that.placeArray[that.placeIndex].name}&floor=${that.floorArray[that.floorIndex].name}&floorId=${that.floorArray[that.floorIndex].id}`
					})

				}
			},

			dateClick() {






				if (this.startindex == false && this.placeindex == false && this.floorindex == false && this.dateindex ==
					false && this.endindex == false) {
					this.dateindex = true;
					this.startindex = false;
					this.endindex = false;
					this.placeindex = false;
					this.floorindex = false;


				}

			},
			startTimeClick() {

				if (this.startindex == false && this.placeindex == false && this.floorindex == false && this.dateindex ==
					false && this.endindex == false) {
					this.dateindex = false;
					this.startindex = true;
					this.endindex = false;
					this.placeindex = false;
					this.floorindex = false;


				}
			},
			endTimeClick() {

				if (this.startindex == false && this.placeindex == false && this.floorindex == false && this.dateindex ==
					false && this.endindex == false) {
					this.dateindex = false;
					this.startindex = false;
					this.endindex = true;
					this.placeindex = false;
					this.floorindex = false;


				}
			},
			placeClick() {

				if (this.startindex == false && this.placeindex == false && this.floorindex == false && this.dateindex ==
					false && this.endindex == false) {
					this.dateindex = false;
					this.startindex = false;
					this.endindex = false;
					this.placeindex = true;
					this.floorindex = false;


				}
			},
			floorClick() {
				if (this.startindex == false && this.placeindex == false && this.floorindex == false && this.dateindex ==
					false && this.endindex == false) {
					this.dateindex = false;
					this.startindex = false;
					this.endindex = false;
					this.placeindex = false;
					this.floorindex = true;


				}
			},

			bindPlacePickerChange(e) {
				let val = e.target.value;
				this.valuePlace = val;
				this.placeIndex = val[0];


				// let id = this.placeArray[this.placeIndex].id;
				// let that = this;
				// uni.request({
				// 	url: `http://${getApp().globalData.http}/app/office/building/floor/list/${id}`,
				// 	// url: `http://82.157.34.130:9901/app/office/building/floor/list/${id}`,

				// 	header: {
				// 		'Content-Type': 'application/json',
				// 		'Authorization': getApp().globalData.token
				// 	},
				// 	success: (res) => {

				// 		let newArray = [];
				// 		res.data.value.map((item) => {

				// 			newArray.push(item);
				// 		})
				// 		that.floorArray = res.data.value;
				// 	}
				// })
			},
			bindDatePickerChange(e) {
				let val = e.target.value;
				this.valueDate = val;
				this.year = this.years[val[0]];
				this.month = this.months[val[1]];
				this.day = this.days[val[2]];
			},
			bindStartTimePickerChange(e) {
				let val = e.target.value;
				this.valueStartTime = val;
				this.startHour = this.hours[val[0]];
				this.startMin = this.mins[val[1]];
			},
			bindEndTimePickerChange(e) {
				let val = e.target.value;
				this.valueEndTime = val;
				this.endHour = this.hours[val[0]];
				this.endMin = this.mins[val[1]];
			},
			bindFloorPickerChange(e) {
				let val = e.target.value;
				this.valueFloor = val;
				this.floorIndex = val[0];


			},
			confirm(name) {
				let val;
				if (name == 'start') {
					val = this.valueStartTime;

					this.startMiddel = val;
				} else if (name == 'end') {
					val = this.valueEndTime;

					this.endMiddel = val;

				} else if (name == 'date') {
					val = this.valueDate;
					this.dateMiddel = val;

				} else if (name == 'floor') {
					val = this.valueFloor;

					this.floorMiddel = val;
				} else if (name == 'place') {

					let id = this.placeArray[this.placeIndex].id;
					let that = this;
					uni.request({
						url: `http://${getApp().globalData.http}/app/office/building/floor/list/${id}`,
						// url: `http://82.157.34.130:9901/app/office/building/floor/list/${id}`,

						header: {
							'Content-Type': 'application/json',
							'Authorization': getApp().globalData.token
						},
						success: (res) => {


							if (res.data.code == 0 && res.data.value.length > 0) {
								let newArray = [];
								res.data.value.map((item) => {
									newArray.push(item);
								})
								that.floorArray = res.data.value;
								that.valueFloor = [0];

								val = that.valuePlace;
								that.placeMiddel = val;

							} else {
								uni.showToast({
									title: '当前楼宇无楼层信息',
									icon: 'none',
									duration: 2000
								})
								that.valuePlace = that.placeMiddel;
								val = that.placeMiddel;
								that.placeIndex = val[0];
								that.valuePlace = [val[0]]

							}


						}
					})

				}
				this.startindex = false;
				this.endindex = false;
				this.dateindex = false;
				this.floorindex = false;
				this.placeindex = false;
			},
			cancel(name) {
				console.log(name);
				let val;
				if (name == 'start') {
					this.valueStartTime = this.startMiddel;
					val = this.startMiddel;
					this.startHour = this.hours[val[0]];
					this.startMin = this.mins[val[1]];
				} else if (name == 'end') {
					this.valueEndTime = this.endMiddel;
					val = this.endMiddel;
					this.endHour = this.hours[val[0]];
					this.endMin = this.mins[val[1]];
				} else if (name == 'date') {
					this.valueDate = this.dateMiddel;
					val = this.dateMiddel;
					this.year = this.years[val[0]];
					this.month = this.months[val[1]];
					this.day = this.days[val[2]];
					console.log(this.day);
				} else if (name == 'floor') {
					this.valueFloor = this.floorMiddel;
					val = this.floorMiddel;

					this.floorIndex = val[0];
					this.valueFloor = [val[0]]
				} else if (name == 'place') {
					this.valuePlace = this.placeMiddel;
					val = this.placeMiddel;
					this.placeIndex = val[0];
					this.valuePlace = [val[0]]
				}
				this.startindex = false;
				this.endindex = false;
				this.dateindex = false;
				this.floorindex = false;
				this.placeindex = false;
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

	.bgc .viewText {
		font-weight: bold;
		padding: calc(100vh * 14/812) calc(750rpx * 16/ 375);
		border-top-left-radius: 30%;
		border-top-right-radius: 30%;
	}

	.head {
		height: calc(100vh *44/812);
	}

	.content {
          background-color: ;
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
		background-color: #F8FDFD;
	}

	.content .main .main-top {
		height: calc(100vh *60/812);
		border-bottom: 1px solid #EDEFF2;

	}

	.content .main .main-center {
        margin: 0;
		padding: 0 calc(750rpx * 16/ 375);
		width: calc(750rpx * 311/ 375);
        background-color: white;

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

	.content .main .main-center .main-center-item .main-center-item-right {
		color: rgba(17, 30, 54, 0.7);
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



	.bgc .pickerDate,
	.pickerDate,
	.pickerEndTime,
	.pickerStartTime,
	.pickerFloor,
	.pickerPlace {
		position: fixed;
		bottom: 0;
		left: 0;
		right: 0;

	}

	.bgc .show {
		height: calc(100vh * 300/812);
		transition: height 0.5s;
	}

	.bgc .hidden {
		height: 0;
		transition: height 0.5s
	}
</style>
