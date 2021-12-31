<template>
	<view class="bgc">
		<web-view @message="getMessage" :style="{width:'100px', height:'100px'}" :src="url">
		</web-view>
		<view class="content">
			<tarbarHeader class="head" style="z-index: 999;">
				<image slot='left' style="width:calc(750rpx * 26.42/ 375);height:calc(750rpx * 28.47/ 375);"
					src="../../static/app/back.svg" @click="backClick"></image>
				<text slot='center'></text>
				<text slot='right'
					style="font-size:calc(750rpx * 17/ 375) ; color: white;margin-left: calc(-750rpx * 220/ 375);">选择工位</text>
			</tarbarHeader>
			<view class="top">
				<view class="top-item" @click="reChose">
					<view style="color: rgba(17, 30, 54, 1);">
						{{date}}
					</view>
					<text style="color: rgba(10, 32, 57, 0.3); ">|</text>
					<view style="color: rgba(19, 194, 194, 1)">
						{{startTime}}--{{endTime}}
					</view>
				</view>
				<view class="top-item">
					<view>
						<view>
							<image src="../../static/app/shangle.svg"
								style="width: calc(750rpx * 14/ 375);height: calc(750rpx * 14/ 375); transform: translateY(10%);margin-left: calc(750rpx * 14/ 375);">
							</image>
						</view>
						<view
							style="width: calc(750rpx * 84/ 375);text-overflow: ellipsis; white-space:nowrap;overflow:hidden;margin-left: 0;">
							{{place}}
						</view>

					</view>
					<view style="margin-left: calc(750rpx * 8/ 375); ">
						<view style="margin-left:calc(750rpx * 5/ 375);">
							{{floor}}
						</view>
						<view>
							<image src="../../static/app/xiala.svg"
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
							:src="show==true ? listImg[1] : listImg[0]"></image>
						<text style="margin-right:calc(750rpx * 36/ 375) ;">列表</text>
					</view>
					<view class="map" @click="mapClick" :class="{'click':!show}">
						<image
							style="width:calc(750rpx * 13.98/ 375);height:calc(750rpx * 11.65/ 375);margin-right:calc(750rpx * 8.02/ 375)"
							:src=" show == true ? mapImg[0] : mapImg[1]"></image>
						<text style="margin-right:calc(750rpx * 36/ 375) ;">地图</text>
					</view>
				</view>
				<view class="center-boxOne" v-show="show">
					<view class="center-boxOne-top" style="background-color: #F8FDFD;">
						<dash></dash>
						<view class="center-boxOne-top-content">
							<view @click="itemClick(item,index,item.enabled)"
								:class="{'click':index==itemIndex,'enabled-false':!item.enabled}"
								class="center-boxOne-top-content-item" v-for="(item,index) in positionArray"
								style="overflow: hidden;text-overflow: ellipsis;white-space: nowrap;">
								{{item.station_number}}
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
								{{item.station_number}}
							</view>
						</view>
					</view>
				</view>

				<view class="center-boxTwo" v-show="!show">

					<!-- 		<view class="map">
						<image src="../../static/app/map.jpg" style="width: 1200px;height: 1200px;"></image>

						<view class="block" v-for="item in positionMapArray"
							:style="{position:'absolute',left:item.x_axis+'px',top:item.y_axis+'px'}">

						</view>
					</view> -->
				</view>
			</view>
			<view class="bottom">
				<view @click="reseverFinshed">
					立即预订（{{itemName}}）
				</view>
			</view>
		</view>
		<view class="reChose" :class="{'show':showIndex,'hidden':!showIndex}">
			<view style="display:flex;justify-content: space-between;background-color: white;" class="main">
				<text style="font-size: calc(750rpx * 14/ 375);padding: calc(750rpx * 9/ 375);"
					@click="cancel">取消</text>
				<text
					style="font-size: calc(750rpx * 14/ 375);padding: calc(750rpx * 9 /375) ;color: rgba(19, 194, 194, 1);"
					@click="confirm">确定</text>
			</view>
			<picker-view style="background-color: white;height: calc(100vh *260/812);text-align: center;"
				@change="PickerChange" :value="choseIndex">
				<picker-view-column>
					<view class="item" v-for="(item,index) in monthDay">{{item}}</view>
				</picker-view-column>
				<picker-view-column>
					<view class="item" v-for="(item,index) in startHourMin">{{item}}</view>
				</picker-view-column>
				<picker-view-column>
					<view class="item" v-for="(item,index) in sign">{{item}}</view>
				</picker-view-column>
				<picker-view-column>
					<view class="item" v-for="(item,index) in endHourMin">{{item}}</view>
				</picker-view-column>
			</picker-view>

		</view>
		<view
			style="position: absolute;top: 0;background-color: rgba(0, 0, 0, 0.5);height: 100vh;width: 100%;display: flex;justify-content: center;align-items: center;"
			v-show="suggestModal">
			<view
				style="height: calc(100vh * 168/812);width:calc(750rpx * 311/375);background-color: white;border-radius: 30rpx;display: flex;flex-direction: column;font-size: calc(750rpx * 16/375);;color: #111E36;font-weight: Regular;">
				<text
					style="height: calc(100vh * 22/812);text-align: center;line-height: calc(100vh * 22/812);margin-top:calc(100vh * 27/812) ;text-align: center;">您选择的工位（{{itemName}}）已经被预订</text>
				<text
					style="height: calc(100vh * 22/812);text-align: center;line-height: calc(100vh * 22/812);text-align: center;">系统推荐您使用空闲工位（{{suggestNumberName}}）</text>
				<text
					style="height: calc(100vh * 22/812);text-align: center;line-height: calc(100vh * 22/812);text-align: center;">是否同意？</text>
				<view style="height: calc(100vh * 48/812) ;display: flex;margin-top:calc(100vh * 27/812) ;">
					<view
						style="width: calc(750rpx * 156/375);text-align: center;line-height:calc(100vh * 48/812) ;border-top: 1px solid #dfe6e9;"
						@click="suggestModalCancel">
						我自己选择
					</view>
					<view
						style="width: calc(750rpx * 156/375);text-align: center;line-height:calc(100vh * 48/812) ;border-left: 1px solid #dfe6e9;border-top: 1px solid #dfe6e9;color: #1ABFC2;"
						@click="suggestModalConfirm">预定 </view>
				</view>
			</view>
		</view>
	</view>



</template>
<script>
	import tarbarHeader from '../../components/common/header/header.vue'
	import tip from '../../components/common/tip/tip.vue'
	import dash from '../../components/common/dash/dash.vue'


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
				floorId: "",
				name: "",
				show: true,
				id: 0,
				positionMapArray: [],
				choseIndex: [0, 0, 0, 0],
				startHourMin: [],
				endHourMin: [],
				monthDay: [],
				sign: ['至'],
				showIndex: false,
				middleIndex: [],
				url: null,
				currentMapPic: null,
				vh: 0,
				height: 0,
				suggestModal: false,
				suggest: "您确定要提前结束吗？",
				suggestId: -1,
				suggestNumberName: "",
				listImg: ['../../static/app/listOne.svg', '../../static/app/listTwo.svg'],
				mapImg: ['../../static/app/mapOne.svg', '../../static/app/mapTwo.svg'],




			}

		},
		props: {
			positionArray: {
				type: Array,
				default: () => {
					return [];
				}
			},
			UsuallyArray: {
				type: Array,
				default: () => {
					return [];
				}
			}
		},
		mounted() {
			this.changeHeight(1)
		},
		onLoad(option) {

			if (!getApp().globalData.token) {
				uni.navigateTo({
					url: "../login/login"
				})
			}
			let that = this;
			uni.getSystemInfo({
				success: function(res) {
					that.vh = res.windowHeight * 0.01;

				}
			});

			uni.hideLoading();

			this.startTime = option.startTime;
			this.endTime = option.endTime;
			this.date = option.date;
			this.place = option.place;
			this.floor = option.floor;
			this.floorId = option.floorId;
			uni.request({
				url: `http://${getApp().globalData.http}/app/office/empty/station/list`,
				// url: `http://82.157.34.130:9901/app/office/empty/station/list`,
				method: 'POST',
				data: {
					'end_time': that.endTime + ":00",
					'start_time': that.startTime + ":00",
					'reserve_date': that.date,
					'floor_id': that.floorId,
				},
				header: {
					'Content-Type': 'application/json',
					'Authorization': getApp().globalData.token
				},
				success: (res) => {
					console.log(getApp().globalData.positionArray);
					that.positionArray = res.data.value;

					console.log(res.data.value);
					console.log(getApp().globalData.positionArray);
					uni.request({
						url: `http://${getApp().globalData.http}/app/office/often/empty/station/list`,
						// url: `http://82.157.34.130:9901/app/office/often/empty/station/list`,
						method: 'POST',
						data: {
							'end_time': that.endTime + ":00",
							'start_time': that.startTime + ":00",
							'reserve_date': that.date,
							'floor_id': that.floorId,
						},
						header: {
							'Content-Type': 'application/json',
							'Authorization': getApp().globalData.token
						},
						success: (res) => {

							that.UsuallyArray = res.data.value;



						}
					})

				}
			})
			uni.request({
				url: `http://${getApp().globalData.http}/app/office/station/map/v2/${that.floorId}`,
				header: {
					'Content-Type': 'application/json',
					'Authorization': getApp().globalData.token,
				},
				success: (res) => {
					var mm = res.data.value;
					if(mm){
						mm.map_url = `http://${getApp().globalData.http}` + res.data.value.map_url;
					}
					that.currentMapPic = mm;
					console.log(that.currentMapPic)
				}
			})

			let month = this.date.split('-')[1];
			let day = this.date.split('-')[2];

			let shour = this.startTime.split(':')[0];
			let smin = this.startTime.split(':')[1];

			let ehour = this.endTime.split(':')[0];
			let emin = this.endTime.split(':')[1];
			for (let i = 1; i <= 12; i++) {
				for (let j = 1; j <= 30; j++) {
					if (i == parseInt(month) && j == parseInt(day)) {
						this.choseIndex[0] = this.monthDay.length;
						console.log(this.choseIndex)
					}
					this.monthDay.push(i + "月" + j + '日')
				}
			}
			for (let i = 0; i <= 23; i++) {
				if (i < 10) {
					i = '0' + i;
				}
				for (let j = 0; j <= 59; j++) {
					if (i == parseInt(shour) && j == parseInt(smin)) {
						this.choseIndex[1] = this.startHourMin.length;

					}
					if (i == parseInt(ehour) && j == parseInt(emin)) {
						this.choseIndex[3] = this.endHourMin.length;

					}

					if (j < 10) {
						j = '0' + j
					}
					this.startHourMin.push(i + ":" + j)
					this.endHourMin.push(i + ":" + j)
				}
			}
			this.middleIndex = this.choseIndex;
			uni.request({
				url: `http://${getApp().globalData.http}/app/office/map/empty/station/list`,
				method: 'POST',
				header: {
					'Content-Type': 'application/json',
					'Authorization': getApp().globalData.token,
				},

				data: {
					'start_time': that.startTime + ":00",
					'end_time': that.endTime + ":00",
					'floor_id': parseInt(that.floorId),
					'reserve_date': that.date,


				},
				success: (res) => {
					console.log(res)
					that.positionMapArray = res.data.value
					this.fiag = true;

					that.covers = [];
					if (that.positionMapArray && that.positionMapArray.length > 0) {
						that.positionMapArray.forEach(function(e, index) {
							var mm = {
								id: e.id,
								content: e.station_number,
								latitude: e.x_axis,
								longitude: e.y_axis,
								enabled: e.enabled,
							}

							that.covers.push(mm);
						})

					}
					setTimeout(function(){
						that.url = '../../static/map/demo.html?pic=' + JSON.stringify(that.currentMapPic) + "&data=" +
							JSON.stringify(that.covers)
							console.log(that.url)
					},300)
					
					
				}

			})

		},

		methods: {
			// getMessage(e) {

			// 	console.log("getMessage触发了");

			// 	//if (JSON.stringify(e.target) != '{}') {
					
			// 		this.itemName = e.detail.data[0].content;
			// 		this.id = e.detail.data[0].id;
			// 	//}
			// },
			getMessage(e) {
			    console.log("getMessage触发了");
			    if(e && e.detail && e.detail.data[0] && e.detail.data[0].id){
			     this.itemName = e.detail.data[0].content;
			     this.id = e.detail.data[0].id;
			    }   
			   },
			backClick() {
				// this.show=true;
				// this.changeHeight(1);//地图回挡住pick-view bug 解决方法	

				uni.navigateBack()
			},
			itemClick(item, index, enabled) {

				if (enabled) {
					this.usuallyItemIndex = -1;
					this.itemName = item.station_number
					this.itemIndex = index;
					this.id = item.id;
				}



			},
			usuallyItemClick(item, index) {
				this.itemIndex = -1;
				this.itemName = item.station_number
				this.usuallyItemIndex = index;
				this.id = item.id;
			},
			mapClick() {
				this.show = false;
				this.changeHeight(395);
				this.height = 395;
			},
			listClick() {

				this.show = true;
				this.changeHeight(1);
				this.height = 1;

				uni.hideLoading();
			},
			changeHeight(height) {

				let that = this;
				var currentWebview = this.$scope.$getAppWebview(); //获取当前web-view
				var wv = currentWebview.children()[0];
				console.log(that.vh);
				wv.setStyle({ //设置web-view距离顶部的距离以及自己的高度，单位为px
					top: that.vh * 35,
					height: height,
					left: 25,
					right: 25,
					zindex: -1,

				})
			},
			reseverFinshed() {
				let that = this;
				if (this.itemIndex == -1 && this.usuallyItemIndex == -1) {
					uni.showModal({
						title: '提示',
						content: '请选择工位',
						showCancel: false,
					});
				} else {
					let obj = {};

					// obj.startTime = this.startTime + ":00";
					// obj.endTime = this.endTime + ":00";
					obj.date = this.date;
					obj.place = this.place;
					obj.floor = parseInt(this.floorId);
					obj.position = this.itemName;
					obj.id = parseInt(this.id);
					let sTime = this.startTime.split(":");
					let eTime = this.endTime.split(":");
					let xH = eTime[0] - sTime[0];
					let xM = eTime[1] - sTime[1];
					const date = new Date();
					let hour = date.getHours() < 10 ? '0' + date.getHours() : date.getHours();
					let mins = date.getMinutes() < 10 ? '0' + date.getMinutes() : date.getMinutes();
					let year = date.getFullYear();
					let month = (date.getMonth() + 1) < 10 ? '0' + (date.getMonth() + 1) : (date.getMonth() + 1);
					let day = date.getDate() < 10 ? '0' + date.getDate() : date.getDate();
					let startTime;
					if (sTime[0] - hour < 0 || ((sTime[0] - hour == 0) && (sTime[1] - mins < 0))) {
						startTime = hour + ":" + mins;
					} else {
						startTime = this.startTime;
					}

					let timeString = startTime.split(":");
					let eHour = parseInt(timeString[0]);
					let eMin = parseInt(timeString[1]);
					let endTime = 0;
					eHour = eHour + xH;
					eMin = eMin + xM;
					if (eMin >= 60) {
						eMin = eMin - 60;
						eHour += 1;
					}
					if (eHour >= 24) {
						eHour = eHour - 24;
					}

					eHour = eHour < 10 ? '0' + eHour : eHour;
					eMin = eMin < 10 ? '0' + eMin : eMin;
					endTime = `${eHour}:${eMin}:00`
					startTime += ":00"


					uni.request({

						url: `http://${getApp().globalData.http}/app/office/reserve`,
						// url: 'http://82.157.34.130:9901/app/office/reserve',
						method: 'POST',
						data: {
							'start_time': startTime,
							'end_time': endTime,
							'floor_id': obj.floor,
							'reserve_date': obj.date,
							'station_id': obj.id,

						},
						header: {
							'Content-Type': 'application/json',
							'Authorization': getApp().globalData.token,
						},
						success: (res) => {
							console.log(res);
							console.log("111")
							if (res.data.code == 0) {
								uni.navigateTo({
									// url: `../login-success/login-success?resever=true&index=1&buttonIndex=1&startTime=${obj.startTime}&endTime=${obj.endTime}&position=${obj.position}`
									url: `../login-success/login-success?index=1&buttonIndex=1`
								});

							} else if (res.data.code == -301) {
								if (res.data.value != null) {
									that.suggestId = res.data.value.id;
									that.suggestNumberName = res.data.value.station_number;
									that.suggestModal = true;
								} else {
									uni.showToast({
										title: '该楼层已无空闲工位请选择其他楼层',
										icon: 'none',
										duration: 2000
									})

								}


							} else {

								uni.showToast({
									title: res.data.message,
									icon: 'none',
									duration: 2000
								})
							}




						}
					})

				}

			},
			reChose() {
				// this.show = true;
				// this.changeHeight(1); //地图回挡住pick-view bug 解决方法
				if (this.height > 1) {
					this.changeHeight(200);
				}

				this.showIndex = true;

			},
			PickerChange(e) {
				this.choseIndex = e.target.value;
				// uni.navigateTo({
				// 	url: `./reservePosition?date=${1}&startTime=${1}&endTime=${1}&place=${1}&floor=${1}&floorId=${1}`
				// })
			},
			cancel() {
				this.choseIndex = this.middleIndex;
				this.showIndex = false;
				if (this.height > 1) {
					this.changeHeight(400);
				}


			},
			confirm() {
				let start = this.startHourMin[this.choseIndex[1]];
				let end = this.endHourMin[this.choseIndex[3]];
				let sh = start.split(':')[0];
				let sm = start.split(":")[1];
				let eh = end.split(':')[0];
				let em = end.split(":")[1];
				let date = this.monthDay[this.choseIndex[0]]
				let month = date.split("月")[0];
				let x = date.split("月")[1];
				let day = x.split('日')[0];

				// let date=this.monthDay[this.choseIndex[0]].splice('月')[0]+this.monthDay[this.choseIndex[0]].splice('月')[1].split('日')[0]

				if ((eh - sh) < 0 || ((eh - sh) == 0 && (em - sm) <= 0)) {
					this.choseIndex = this.middleIndex;
					uni.showToast({
						title: '结束时间需大于开始时间',
						icon: 'none',
						duration: 2000
					});
				} else {
					this.startTime = start;
					this.endTime = end;
					this.date = this.date.split('-')[0] + "-" + month + '-' + day;
					this.showIndex = false;
					let that = this;

					uni.showLoading({
						title: '加载中'
					})
					// uni.navigateTo({
					// 	url: `./reservePosition?date=${that.date}&startTime=${that.startTime}&endTime=${that.endTime}&place=${that.place}&floor=${that.floor}&floorId=${that.floorId}`
					// })
					uni.request({
						url: `http://${getApp().globalData.http}/app/office/empty/station/list`,
						// url: `http://82.157.34.130:9901/app/office/empty/station/list`,
						method: 'POST',
						data: {
							'end_time': that.endTime + ":00",
							'start_time': that.startTime + ":00",
							'reserve_date': that.date,
							'floor_id': that.floorId,
						},
						header: {
							'Content-Type': 'application/json',
							'Authorization': getApp().globalData.token
						},
						success: (res) => {
							console.log(getApp().globalData.positionArray);
							that.positionArray = res.data.value;

							console.log(res.data.value);
							console.log(getApp().globalData.positionArray);
							uni.request({
								url: `http://${getApp().globalData.http}/app/office/often/empty/station/list`,
								// url: `http://82.157.34.130:9901/app/office/often/empty/station/list`,
								method: 'POST',
								data: {
									'end_time': that.endTime + ":00",
									'start_time': that.startTime + ":00",
									'reserve_date': that.date,
									'floor_id': that.floorId,
								},
								header: {
									'Content-Type': 'application/json',
									'Authorization': getApp().globalData.token
								},
								success: (res) => {

									that.UsuallyArray = res.data.value;



								}
							})

						}
					})
					uni.request({
						url: `http://${getApp().globalData.http}/app/office/station/map/v2/${that.floorId}`,
						header: {
							'Content-Type': 'application/json',
							'Authorization': getApp().globalData.token,
						},
						success: (res) => {
							var mm = res.data.value;
							if(mm){
								mm.map_url = `http://${getApp().globalData.http}` + res.data.value.map_url;
							}
							that.currentMapPic = mm;
							console.log(that.currentMapPic)
						}
					})


					uni.request({
						url: `http://${getApp().globalData.http}/app/office/map/empty/station/list`,
						method: 'POST',
						header: {
							'Content-Type': 'application/json',
							'Authorization': getApp().globalData.token,
						},

						data: {
							'start_time': that.startTime + ":00",
							'end_time': that.endTime + ":00",
							'floor_id': parseInt(that.floorId),
							'reserve_date': that.date,


						},
						success: (res) => {

							console.log(res)
							that.positionMapArray = res.data.value
							this.fiag = true;

							that.covers = [];
							if (that.positionMapArray && that.positionMapArray.length > 0) {
								that.positionMapArray.forEach(function(e, index) {
									var mm = {
										id: e.id,
										content: e.station_number,
										latitude: e.x_axis,
										longitude: e.y_axis,
										enabled: e.enabled,
									}

									that.covers.push(mm);
								})

							}
							setTimeout(function(){
								that.url = '../../static/map/demo.html?pic=' + JSON.stringify(that.currentMapPic) + "&data=" +
									JSON.stringify(that.covers)
									console.log(that.url)
							},300)
								
							
						}

					})
					if (this.height > 1) {
						this.changeHeight(395);
					}
					setTimeout(function() {
						uni.hideLoading();
					}, 1500)
				}
			},
			suggestModalCancel() {
				this.suggestModal = false;
			},
			suggestModalConfirm() {
				this.id = this.suggestId;
				this.reseverFinshed();
			}
		},
		components: {
			tarbarHeader,
			tip,
			dash

		},


	}
</script>

<style>
	.bgc {
		overflow: hidden;
		position: relative;
		height: 100vh;
		background-color: rgba(241, 242, 246, 1);


	}

	.bgc .reChose {
		position: absolute;
		bottom: 0;
		left: 0;
		right: 0;
		z-index: 99999;

	}

	.bgc .reChose .main {
		font-weight: bold;
		padding: calc(100vh * 14/812) calc(750rpx * 16/ 375);
		border-top-left-radius: 30%;
		border-top-right-radius: 30%;
	}

	.bgc .reChose.show {
		height: calc(100vh * 300/812);
		transition: height 0.5s;
	}

	.bgc .reChose.hidden {
		height: 0;
		transition: height 0.5s
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

		display: flex;
		flex-wrap: wrap;
		align-content: flex-start;

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
		flex-shrink: 0;
	}

	.content .center .center-boxOne .center-boxOne-top .center-boxOne-top-content .center-boxOne-top-content-item.click {
		border-color: rgba(251, 178, 88, 1);
		background-color: rgba(255, 234, 167, 0.5);
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
		overflow: hidden;
		text-overflow: ellipsis;
		background-color: white;
	}

	.content .center .center-boxOne .center-boxOne-bottom .center-boxOne-bottom-centent .center-boxOne-bottom-centent-item:first-child::before {
		content: '';
		display: block;
		width: calc(750rpx * 8/ 375);
		height: calc(750rpx * 8/ 375);
		border-radius: 50%;
		background-color: #1ABFC2;
		position: absolute;
		left: 75%;
		top: 40%;
	}

	.content .center .center-boxOne .center-boxOne-bottom .center-boxOne-bottom-centent .center-boxOne-bottom-centent-item.click {
		border-color: rgba(251, 178, 88, 1);
		background-color: rgba(255, 234, 167, 0.5);
		color: rgba(251, 178, 88, 1);
	}

	.content .center .center-boxTwo {
		overflow: scroll;
		width: calc(750rpx * 343/ 375);
		height: calc(100vh * 423/812);
		border-bottom-right-radius: calc(750rpx * 30/375);
		border-bottom-left-radius: calc(750rpx * 30/ 375);
		background-color: #13C2C2;

	}

	/* 	.content .center .center-boxTwo .map {
		position: relative;
	}
.content .center .center-boxTwo .map .block{
	display: block;
	width: 10px;
	height: 10px;
	background-color: #007AFF;
} */
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

	.enabled-false {
		background-color: #dfe6e9;
		color: #b2bec3;
	}
</style>
