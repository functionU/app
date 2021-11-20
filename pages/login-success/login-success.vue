<template>
	<view>
		<view class="bgc">
			<view class="content">
				<view class="content-top">
					<tarbarHeader class="head" @newsClick="newsClickListen">
						<text slot="center"></text>
					</tarbarHeader>
					<tarbar :clickIndex="index" @tarbarClick='tarbarListen'></tarbar>
					<view class="content-main" v-show="index==0">
						<view class="firstShow">
							<view class="firstShow-top">
								<view class="left">
									<image src="../../static/app/icon-jiankang@2X.png"
										style="width: calc(750rpx * 32/ 375);height: calc(750rpx * 32/ 375);"></image>
								</view>
								<view class="right">
									<text style="font-size: calc(750rpx * 16/ 375);font-weight: bold;">您已连续⼯作1⼩时！</text>
									<text
										style="font-size: calc(750rpx * 12/ 375); color: #666D7F;">在工位上的时间：6小时30分钟</text>
								</view>
							</view>
							<view class="firstShow-center">
								<view class="top-box">
									<view class="item" v-for="item in boxArray"
										:class="{'colorOne':item==0,'colorTwo':item==1,'colorThree':item==2}">

									</view>
								</view>
								<view class="center-box">
									<view>
										<image src="../../static/app/icon-zhengque.png"
											style="width: calc(750rpx * 10/ 375);height: calc(750rpx * 9.8/ 375); margin-right: calc(750rpx * 4/ 375) ;">
										</image>
										8.20
									</view>
									<view>
										12:00
									</view>
									<view>
										15:00
									</view>
									<view>
										18:50
										<image src="../../static/app/icon-title@2X.png"
											style="width: calc(750rpx * 10/ 375);height: calc(750rpx * 9.8/ 375); margin-left: calc(750rpx * 4/ 375)">
										</image>
									</view>
								</view>
							</view>
							<view class="firstShow-bottom">
								<text>健康</text>
								<text>无人</text>
								<text>久坐</text>
							</view>
							<view class="firstShow-bottom-bottom">
								<view class="top-box">
									<view class="left">
										<image src="../../static/app/icon-title@2X.png"
											style="width: calc(750rpx * 10/ 375);height: calc(750rpx * 9.8/ 375); margin-right: calc(750rpx * 4/ 375)">
											<text>健康指数：<b> {{grade}}分</b></text>

									</view>
									<view class="right" @click="warnClick"
										style="font-size: calc(750rpx * 12/ 375);color: rgba(10, 32, 57, 0.5);">
										<image src="../../static/app/icon-title@2X.png"
											style="width: calc(750rpx * 10/ 375);height: calc(750rpx * 9.8/ 375); margin-right: calc(750rpx * 4/ 375)">
											计数规则

									</view>
								</view>
								<view class="bottom-box">

									<view class="top" style="position: relative;">
										<view class="radius"
											:style="{'position':'absolute','left':getLeft,'top':'-50%','background-image':getColor,'border-radius':'50%'}"
											style="width: calc(750rpx * 18/ 375) ;height:calc(750rpx * 18/ 375);background-color: white;padding:calc(750rpx * 4/ 375) ;box-sizing: border-box;">
											<view
												style="width: 100%;height: 100%; border-radius: 50%;background-color:white;">
												<image :src="getSrc"
													style="width:calc(750rpx * 36/ 375);height:calc(750rpx * 31/375);position: absolute;top: -170%;left: -40%;">
												</image>
											</view>

										</view>
										<view class="long">

										</view>
										<view class="long">

										</view>
										<view class="long">
										</view>
									</view>
									<view class="bottom"
										style="display: flex; justify-content: space-between;font-size:calc(750rpx * 12/ 375) ;">
										<text>久坐</text>
										<text>健康</text>
									</view>
								</view>
							</view>
						</view>
						<view class="secondShow">
							<tip>
								<view slot='right' class="button">
									<text @click="weekClick" :class="{'click':secondShowButtonIndex=='week'}">每周</text>
									<text @click="monthClick"
										:class="{'click':secondShowButtonIndex=='month'}">每月</text>
								</view>
							</tip>
							<view class="secondShow-box">
								<qiun-data-charts type="line" :chartData="chartData" :errorShow="false"
									background="none" :reshow="index==0" />
							</view>

						</view>
					</view>
					<view class="content-main" v-show="index==2">
						<view class="safeFirstShow">
							<tip :item="{name:'今日用电'}">
								<text slot="right"></text>
							</tip>
							<view class="safeFirstShow-box">
								<qiun-data-charts type="line" :chartData="SafeChartData" :errorShow="false"
									background="none" :reshow="index==2" />
							</view>
						</view>
						<view class="safeSecondShow">
							<tip>
								<view slot='right' class="SafeButton">
									<text @click="safeWeekClick"
										:class="{'click':secondShowSafeButtonIndex=='week'}">每周</text>
									<text @click="safeMonthClick"
										:class="{'click':secondShowSafeButtonIndex=='month'}">每月</text>
								</view>
							</tip>
							<view class="safeSecondShow-box">
								<qiun-data-charts type="line" :chartData="SafeChartData" :errorShow="false"
									background="none" :reshow="index==2" />
							</view>

						</view>
					</view>
					<view class="content-main" v-show="index==1">
						<doubleButton :number='getButtonIndex' @doubleButtonClick='doubleButtonListen'></doubleButton>
						<view class="show-first" v-show="buttonIndex==0">
							<view class="reserveText">
								{{station}}
							</view>
							<view class="reserveFirst">
								<view class="reserveFirst-left" :style="{backgroundImage:'url(' + item.src + ')'}">

								</view>
								<view class="reserveFirst-center">
									<text>{{item.english}}</text>
									<text>{{item.chinese}}</text>
								</view>

								<view class="reserveFirst-right" @click="powerButtonClick"
									:class="{'reserveFirst-right-click':powerButton}">
									<view
										:class="{'reserveFirst-right-radius':true,'reserveFirst-right-radius-click':powerButton}">
									</view>
									<text :class="{'close':!powerButton}">关</text>
									<text style="visibility: hidden;">1</text>
									<text :class="{'open':powerButton}">开</text>
								</view>
							</view>
							<view class="reserveSecond">
								<tip :item="{name:'今日用电量（kwh）'}">
									<view slot="right" class="right" @click="sumUseClick">
										<text>用电统计</text>
										<image src="../../static/app/map-xianzhong@2X.png"
											style="width:calc(750rpx * 26.42/ 375);height:calc(750rpx * 28.47	/ 375);">
										</image>
									</view>
								</tip>
								<view class="reserveSecond-box">

									<qiun-data-charts type="column" :chartData="reserveChartData" background="none"
										:reshow="index==1&&buttonIndex==0" />

								</view>
							</view>
						</view>
						<view class="show-second" v-show="buttonIndex==1">
							<view v-show="!resever">
								<view class="reserveFirst">

									<view class="left">
										<text>快速预约</text>
										<text>选择工位和时间进行使用～</text>
									</view>
									<view class="right" @click="reserveClick">
										<image src="../../static/app/yuyue@2X.png"></image>
									</view>
								</view>
								<view class="reserveSecond" style="height:auto">

									<tip :item="{name:'我的预约'}">
										<text slot='right'
											style="font-size:calc(750rpx * 24/ 375);font-weight: bold;color: black;"></text>
									</tip>
									<view class="reserveSecond-img" style="height:auto;">

										<view style="display: flex;flex-direction:column;align-items: center;"
										 v-show="!infoTag"
											>
											<image src="../../static/app/nodata@2X.png"
												style="width: calc(750rpx * 120/ 375);height: calc(100vh * 120/812); border:calc(750rpx * 1/ 375)  dashed rgba(230, 231, 232, 1);">
												<text style="color: rgba(10, 32, 57, 0.5);">还没有预订工位～</text>
												<text style="color: rgba(26, 191, 194, 1);"
													@click="reserveClick">点击预定</text>
										</view>
										<view  v-show="infoTag"
											style="display: flex;flex-direction: column;align-items: flex-start;font-size: calc(750rpx * 14/ 375);color:gray;text-align: center;">
											<view>
												<image src="../../static/app/info@2X.png"
													style="width:calc(750rpx * 14/ 375);height:calc(750rpx * 14/ 375);margin-right:calc(750rpx * 8/ 375);">
												</image>
												<text>{{listObj.office_building_name}} {{listObj.floor_name}}</text>
											</view>
											<view style="margin-top: calc(750rpx * 12/ 375);">
												<image src="../../static/app/info@2X.png"
													style="width:calc(750rpx * 14/ 375);height:calc(750rpx *14/ 375);margin-right:calc(750rpx * 8/ 375);">
												</image>
												<text>{{listObj.reserve_date}}{{listObj.start_time}}-{{listObj.end_time}}</text>
											</view>
											<view @click="stopInfo(listObj.id)"
												style="border-radius:calc(750rpx * 30/ 375);width: calc(750rpx * 108/ 375);height: calc(100vh * 32/812);margin-left: 30%;margin-top: %;border:calc(750rpx * 1/ 375)  solid  #1ABFC2;line-height:  calc(100vh * 32/812);color: #1ABFC2;">
												取消预约
											</view>
										</view>

									</view>

								</view>
							</view>
							<view v-show="resever">
								<view class="reserveFirst" style="height: calc(100vh * 83/812);">
									<view class="right">
										<image src="../../static/app/chazuo@2X.png"></image>
									</view>
									<view class="left">
										<text>{{infoObj.station_number}}</text>
										<text>{{infoObj.start_time}}～{{infoObj.end_time}}</text>
									</view>

								</view>
								<view class="beforeSign" v-show="!sign">
									<view
										style="	width: calc(750rpx * 343/ 375);height: calc(100vh * 148/812);display: flex; margin-top: calc(100vh * 20/812);">
										<view @click="signIn(infoObj.id,infoObj.device_number)"
											style="	width: calc(750rpx * 164/ 375);height: calc(100vh * 148/812); margin-left:calc(750rpx * 25/ 375);border-radius:calc(750rpx * 30/ 375); display: flex;flex-direction: column;	justify-content: space-around;align-items: center;background-color: rgba(253, 254, 255, 1);">
											<image src="../../static/app/btn-qiandao@2X.png"
												style="width: calc(750rpx * 76/ 375);height: calc(100vh * 76/812);">
											</image>
											<text>扫码签到</text>
										</view>
										<view @click="reserveStop(infoObj.id)"
											style="	width: calc(750rpx * 164/ 375);height: calc(100vh * 148/812); margin-left:calc(750rpx * 15/ 375);border-radius:calc(750rpx * 30/ 375); display: flex;flex-direction: column;justify-content: space-around;align-items: center;background-color: rgba(253, 254, 255, 1);">
											<image src="../../static/app/btn-quxiao@2X.png"
												style="width: calc(750rpx * 76/ 375);height: calc(100vh * 76/812);">
											</image>
											<text>取消预约 </text>
										</view>

									</view>
									<view class="reserveSecond" style="height: calc(100vh * 267/812);">
										<tip :item="{name:'我的预约'}">
											<text slot='right'
												style="font-size:calc(750rpx * 24/ 375) ;color: black;"></text>
										</tip>
										<view class="reserveSecond-box">
<!-- 										<view  v-show="infoTag"
											style="display: flex;flex-direction: column;align-items: flex-start;font-size: calc(750rpx * 14/ 375);color:gray;text-align: center;margin-left: calc(750rpx * 24/ 375);margin-top: calc(100vh * 14/ 812);">
											<view>
												<image src="../../static/app/info@2X.png"
													style="width:calc(750rpx * 14/ 375);height:calc(750rpx * 14/ 375);margin-right:calc(750rpx * 8/ 375);">
												</image>
												<text>{{listObj.office_building_name}} {{listObj.floor_name}}</text>
											</view>
											<view style="margin-top: calc(750rpx * 12/ 375);">
												<image src="../../static/app/info@2X.png"
													style="width:calc(750rpx * 14/ 375);height:calc(750rpx *14/ 375);margin-right:calc(750rpx * 8/ 375);">
												</image>
												<text>{{listObj.reserve_date}}{{listObj.start_time}}-{{listObj.end_time}}</text>
											</view>
											<view @click="stopInfo(listObj.id)"
												style="border-radius:calc(750rpx * 30/ 375);width: calc(750rpx * 108/ 375);height: calc(100vh * 32/812);margin-left: 30%;margin-top: %;border:calc(750rpx * 1/ 375)  solid  #1ABFC2;line-height:  calc(100vh * 32/812);color: #1ABFC2;">
												取消预约
											</view>
										</view> -->
										</view>


									</view>
								</view>
								<view class="afterSign" v-show="sign">
									<view class="reserveFirst" style="background-color: white;" >
										<view class="reserveFirst-left"
											:style="{backgroundImage:'url(' + item.src + ')'}">

										</view>
										<view class="reserveFirst-center ">
											<text>{{item.english}}</text>
											<text>{{item.chinese}}</text>
										</view>

										<view class="reserveFirst-right" @click="powerButtonClick"
											:class="{'reserveFirst-right-click':powerButton}">
											<view
												:class="{'reserveFirst-right-radius':true,'reserveFirst-right-radius-click':powerButton}">
											</view>
											<text :class="{'close':!powerButton}">关</text>
											<text style="visibility: hidden;">1</text>
											<text :class="{'open':powerButton}">开</text>
										</view>
									</view>
									<view 
										style="display: flex; margin-left: calc(-750rpx * 8/ 375);margin-top:calc(100vh * 20/812) ;">
										<view @click="addUseTime"
											style="	width: calc(750rpx * 164/ 375);height: calc(100vh * 148/812); margin-left:calc(750rpx * 25/ 375);border-radius:calc(750rpx * 30/ 375); display: flex;flex-direction: column;	justify-content: space-around;align-items: center;background-color: rgba(253, 254, 255, 1);">
											<image src="../../static/app/btn-qiandao@2X.png"
												style="width: calc(750rpx * 76/ 375);height: calc(100vh * 76/812);">
											</image>
											<text>延长使用</text>
										</view>
										<view @click="useStop(infoObj.id)"
											style="	width: calc(750rpx * 164/ 375);height: calc(100vh * 148/812); margin-left:calc(750rpx * 15/ 375);border-radius:calc(750rpx * 30/ 375); display: flex;flex-direction: column;justify-content: space-around;align-items: center;background-color: rgba(253, 254, 255, 1);">
											<image src="../../static/app/btn-quxiao@2X.png"
												style="width: calc(750rpx * 76/ 375);height: calc(100vh * 76/812);">
											</image>
											<text>结束使用 </text>
										</view>

										<view class="picker" v-show="addUse"
											style="position: fixed;bottom: 0;left: 0;right: 0;">
											<view
												style="display:flex;justify-content: space-between;background-color: white; ">
												<text @click="addCancal"
													style="font-size: calc(750rpx * 14/ 375);padding: calc(750rpx * 9/ 375);">取消</text>
												<text>延长使用</text>
												<text @click="addSubmit"
													style="font-size: calc(750rpx * 14/ 375);padding: calc(750rpx * 9 /375) ;color: rgba(19, 194, 194, 1);">确定</text>
											</view>
											<picker-view
												style="width: calc(750rpx * 375/ 375);height: calc(100vh * 270/812);"
												class="picker-item" @change="addUseTimeChose" :value="addIndex">
												<picker-view-column>
													<view class="item" v-for="(item,index) in timeArray">{{item}}分钟
													</view>
												</picker-view-column>
											</picker-view>

										</view>
									</view>
									<view class="reserveSecond" style="height: calc(100vh * 166/812);">
										<tip :item="{name:'我的预约'}">
											<text slot='right'
												style="font-size:calc(750rpx * 24/ 375) ;color: black;">{{listObj.position}}</text>
										</tip>
										<view class="reserveSecond-box">
<!-- 										<view  v-show="infoTag"
											style="display: flex;flex-direction: column;align-items: flex-start;font-size: calc(750rpx * 14/ 375);color:gray;text-align: center;margin-left: calc(750rpx * 24/ 375);margin-top: calc(100vh * 14/ 812);">
											<view>
												<image src="../../static/app/info@2X.png"
													style="width:calc(750rpx * 14/ 375);height:calc(750rpx * 14/ 375);margin-right:calc(750rpx * 8/ 375);">
												</image>
												<text>{{listObj.office_building_name}} {{listObj.floor_name}}</text>
											</view>
											<view style="margin-top: calc(750rpx * 12/ 375);">
												<image src="../../static/app/info@2X.png"
													style="width:calc(750rpx * 14/ 375);height:calc(750rpx *14/ 375);margin-right:calc(750rpx * 8/ 375);">
												</image>
												<text>{{listObj.reserve_date}}{{listObj.start_time}}-{{listObj.end_time}}</text>
											</view>
											<view @click="stopInfo(listObj.id)"
												style="border-radius:calc(750rpx * 30/ 375);width: calc(750rpx * 108/ 375);height: calc(100vh * 32/812);margin-left: 30%;margin-top: %;border:calc(750rpx * 1/ 375)  solid  #1ABFC2;line-height:  calc(100vh * 32/812);color: #1ABFC2;">
												取消预约
											</view>
										</view> -->
									
										</view>


									</view>
								</view>
							</view>

						</view>

					</view>
					<view class="content-main" v-show="index==3">
						<view class="firstOffice">
							<tip></tip>
							<view class="environment">
								<environmentItem v-for="(item,index) in environmentes" class="environment-item">
								</environmentItem>

							</view>
							<view class="firstOffice-tip">
								建议配备智能硬件，更准确的知晓身边的工作环境～
							</view>
						</view>
						<view class="secondOffice">
							<tip :item="{name:'天气预报',place:'上海市 静安区'}"></tip>
							<dash></dash>
							<view>
								<view class="secondOffice-item" style="margin-left:  calc(750rpx * 24/ 375);">
									<view>
										<image style="width: calc(750rpx * 12/ 375);height: calc(100vh * 12/812);"
											src="../../static/app/map-xianzhong@2X.png" mode=""></image>
										<text style="margin-left:  calc(750rpx * 13/ 375);">今天-阵雨</text>
									</view>
									<text style="margin-right:  calc(750rpx * 24/ 375);">12°/8°</text>
								</view>
							</view>
						</view>

					</view>
				</view>
			</view>
		</view>
		<warn class="warnning" @confirmClick="confirmListen" :class="{'warn-hidden':!warnShow,'warn-show':warnShow}">
		</warn>
	</view>




</template>\
<script>
	import tarbar from '../../components/common/tarbar/tarbar.vue'
	import tarbarHeader from '../../components/common/header/header.vue'
	import environmentItem from '../../components/common/environment/environmentItem.vue'
	import tip from '../../components/common/tip/tip.vue'
	import dash from '../../components/common/dash/dash.vue'
	import doubleButton from '../../components/common/doubleButton/doubleButton.vue'
	import divView from '../../components/common/divView/divView.vue'
	import warn from '../../components/common/warn/warn.vue'
	export default {
		data() {
			return {
				index: 0,
				buttonIndex: 0,
				powerButton: false,
				warnShow: false,
				secondShowButtonIndex: 'week',
				secondShowSafeButtonIndex: 'week',
				station: 'YJ051',
				resever: false,
				item: {
					english: 'On',
					chinese: '电源开启',
					src: '../../static/app/chazuo@2X.png'
				},
				environmentes: [{
					name: '温度',
					src: '../../static/app/icon-zhengque.png',
					number: '11.11',
					quality: '差'
				}, {
					name: '温度',
					src: '../../static/app/icon-zhengque.png',
					number: '11.11',
					quality: '差'
				}, {
					name: '温度',
					src: '../../static/app/icon-zhengque.png',
					number: '11.11',
					quality: '差'
				}, {
					name: '温度',
					src: '../../static/app/icon-zhengque.png',
					number: '11.11',
					quality: '差'
				}, {
					name: '温度',
					src: '../../static/app/icon-zhengque.png',
					number: '11.11',
					quality: '差'
				}, {
					name: '温度',
					src: '../../static/app/icon-zhengque.png',
					number: '11.11',
					quality: '差'
				}],
				chartData: {
					categories: ['', '', '', '', '', '', '周一', '', '', '', '', '', '', '周二', '', '', '', '', '', '',
						'周三', '', '', '', '', '', '', '周四', '', '', '', '', '', '', '周五'
					],
					series: [{
						name: '指数',
						data: [1, 27, 21, 24, 8, 14, 52, 33, 27, 21, 24, 8, 14, 52, 33, 27, 21, 24, 8, 14, 52,
							33, 27, 21, 24, 8, 14, 52, 33, 27, 21, 24, 8, 14, 52
						],
						color: '#13C2C2',
					}]
				},
				SafeChartData: {
					categories: ['', '', '', '', '', '', '周一', '', '', '', '', '', '', '周二', '', '', '', '', '', '', '周三',
						'', '', '', '', '', '', '周四', '', '', '', '', '', '', '周五',
					],
					series: [{
						name: '',
						data: [1, 4, 5, 6, 7, 8, 1, 1, 4, 5, 6, 7, 8, 1, 1, 4, 5, 6, 7, 8, 1, 1, 4, 5, 6, 7, 8, 1,
							1, 4, 5, 6, 7, 8,
						],
						color: '#13C2C2',
					}]
				},
				reserveChartData: {

					categories: ['周一', '周二', '周三', '周四', '周五'],
					series: [{
						name: '',
						data: [33, 27, 21, 24, 8],
						color: '#FFF',
					}]
				},
				boxArray: [0, 1, 2, 0, 2, 1, 0, 2, 2, 1, 1, 0, 0, 1, 1, 2, 2, 2],
				grade: 73,
				color: '#007AFF',
				reserve: false,
				sign: false,
				timeArray: [15, 30, 45, 60, 90, 120],
				addUse: false,
				addIndex: [0],
				e: [0],
				infoObj: {},
				listObj: {},
				infoTag: false,
				signTag:false,


			}
		},
		onLoad(option) {
			let that = this;
			if (option.index) {
				this.resever = option.resever;
				this.buttonIndex = parseInt(option.buttonIndex);
				this.index = parseInt(option.index);
				this.startTime = option.startTime;
				this.endTime = option.endTime;
				this.position = option.position;
			}
			uni.request({
				// url: 'http://192.168.1.238:9900/app/office/reserve/station/info',
				url: 'http://82.157.34.130:9901/app/office/reserve/station/list',
				header: {
			
					'Authorization': getApp().globalData.token,
				},
				success: (res) => {
					console.log(res)
					if (Array.isArray(res.data.value) && res.data.value.length != 0) {
						that.listObj = res.data.value[0];
						that.infoTag = true;
						// that.resever = true;
						console.log(that.listObj);
					}
			
				}
			})
			uni.request({
				// url: 'http://192.168.1.238:9900/app/office/reserve/station/info',
				url: 'http://82.157.34.130:9901/app/office/reserve/station/info',
				header: {
					'Authorization': getApp().globalData.token,
				},
				success: (res) => {
					console.log(res)
					if (res.data.value) {
						that.resever = true;
						that.sign = false;
						that.infoObj = res.data.value;
						that.sign=res.data.value.sign_status;
						console.log(that.infoObj)
					}

				}
			})





		},
		onShow() {

		},
		methods: {
			tarbarListen(index) {
				this.index = index;

			},
			doubleButtonListen(index) {

				this.buttonIndex = index;

			},
			powerButtonClick() {
				this.powerButton = !this.powerButton;
				if (!this.powerButton) {
					this.item = {
						english: 'On',
						chinese: '电源开启',
						src: '../../static/app/chazuo@2X.png'
					}
				} else if (this.powerButton) {
					this.item = {
						english: 'Off',
						chinese: '电源关闭',
						src: '../../static/app/chazuo@2X.png'
					}
				}

			},
			confirmListen() {
				this.warnShow = false;
			},
			newsClickListen() {

				uni.navigateTo({
					url: '../newsWarn/newsWarn'
				})
			},
			monthClick() {
				this.secondShowButtonIndex = 'month';
				this.chartData = {
					categories: ['', '', '', '', '', '', '周一', '', '', '', '', '', '', '周二', '', '', '', '', '', '',
						'周三', '', '', '', '', '', '', '周四', '', '', '', '', '', '', '周五',
					],
					series: [{
						name: '指数',
						data: [55, 88, 33, 44, 61, 22, 18, 55, 88, 33, 44, 61, 22, 18, 55, 88, 33, 44, 61, 22,
							18, 55, 88, 33, 44, 61, 22, 18, 55, 88, 33, 44, 61, 22, 18,
						],
						color: '#13C2C2',
					}]
				};
			},
			weekClick() {
				this.secondShowButtonIndex = 'week';
				this.chartData = {
					categories: ['', '', '', '', '', '', '周一', '', '', '', '', '', '', '周二', '', '', '', '', '', '',
						'周三', '', '', '', '', '', '', '周四', '', '', '', '', '', '', '周五'
					],
					series: [{
						name: '指数',
						data: [1, 27, 21, 24, 8, 14, 52, 33, 27, 21, 24, 8, 14, 52, 33, 27, 21, 24, 8, 14, 52,
							33, 27, 21, 24, 8, 14, 52, 33, 27, 21, 24, 8, 14, 52
						],
						color: '#13C2C2',
					}]
				}
			},
			safeMonthClick() {
				this.secondShowSafeButtonIndex = 'month';
				this.SafeChartData = {
					categories: ['', '', '', '', '', '', '周一', '', '', '', '', '', '', '周二', '', '', '', '', '', '',
						'周三', '', '', '', '', '', '', '周四', '', '', '', '', '', '', '周五',
					],
					series: [{
						name: '指数',
						data: [55, 88, 33, 44, 61, 22, 18, 55, 88, 33, 44, 61, 22, 18, 55, 88, 33, 44, 61, 22,
							18, 55, 88, 33, 44, 61, 22, 18, 55, 88, 33, 44, 61, 22, 18,
						],
						color: '#13C2C2',
					}]
				};

			},
			safeWeekClick() {
				this.secondShowSafeButtonIndex = 'week';
				this.SafeChartData = {
					categories: ['', '', '', '', '', '', '周一', '', '', '', '', '', '', '周二', '', '', '', '', '', '',
						'周三', '', '', '', '', '', '', '周四', '', '', '', '', '', '', '周五'
					],
					series: [{
						name: '指数',
						data: [1, 27, 21, 24, 8, 14, 52, 33, 27, 21, 24, 8, 14, 52, 33, 27, 21, 24, 8, 14, 52,
							33, 27, 21, 24, 8, 14, 52, 33, 27, 21, 24, 8, 14, 52
						],
						color: '#13C2C2',
					}]
				};
			},
			reserveClick() {

				uni.navigateTo({
					url: '../reserve/reserve'
				})
			},
			sumUseClick() {
				uni.navigateTo({
					url: '../sumUse/sumUse'
				})
			},
			warnClick() {
				this.warnShow = true;

			},
			stopInfo(id) {
				let that = this;
				uni.request({
					// url: `http://192.168.1.238:9900/app/office/advance/end/${id}`,
					url: `http://82.157.34.130:9901/app/office/delete/reserve/${id}`,
					method: 'DELETE',
					header: {
						'Authorization': getApp().globalData.token,
					},
					success: (res) => {
						
						console.log(res);
						that.infoTag = false;
						that.listObj = {};
					}
				})
			},
			reserveStop(id) {
				let that = this;
				console.log(id)
				uni.showModal({
					title: '提示',
					content: '您确定要取消预约吗？',
					success: (res) => {
						if (res.confirm) {
							this.resever = !this.resever;
							uni.request({
								// url: 'http://192.168.1.238:9900//app/office/advance/end/{id}',
								url: `http://82.157.34.130:9901/app/office/delete/reserve/${id}`,
								method: 'DELETE',
								// header: {

								// 	'Authorization': getApp().globalData.token,
								// },
								success: (res) => {
									console.log(res)
									that.infoObj = {};



								}
							})
						}

					},
				});


			},
			signIn(id, devNumber) {
				// uni.scanCode({
				// 	success: function(res) {
				// 		console.log('条码类型：' + res.scanType);
				// 		console.log('条码内容：' + res.result);
				// 	}
				// });
				uni.request({
					// url: `http://192.168.1.238:9900/app/office/delete/reserve/${id}`,
					url: `http://82.157.34.130:9901/app/office/sign/reserve?id=${id}&deviceNumber=${devNumber}`,
					header: {
						'Authorization': getApp().globalData.token,
					},
					success: (res) => {
					
						if (res.data.code == 0) {
							this.sign = true;
						}

					}
				})

			},
			addUseTime() {
				this.addUse = true;

			},
			useStop(id) {
				uni.showModal({
					title: '提示',
					content: '您确定要提前结束吗？',
					success: (res) => {
						console.log(res);
						this.sign = !this.sign;
						this.resever = !this.resever;
						uni.request({
							// url: `http://192.168.1.238:9900/app/office/delete/reserve/${id}`,
							url: `http://82.157.34.130:9901/app/office/advance/end/${id}`,
							header: {
								'Authorization': getApp().globalData.token,
							},
							success: (res) => {
								console.log(res);
							}
						})
					},
				});
			},
			addCancal() {
				this.addUse = false;

			},
			addUseTimeChose(e) {
				this.e = e.target.value;
			},

			addSubmit() {
				this.addIndex = this.e;
				this.addUse = false;
				let minute = this.timeArray[this.addIndex];
				let id = this.infoObj.id;
				uni.request({
					// url: `http://192.168.1.238:9900/app/office/extend/use?id=1&minute=${minute}`,
					url: `http://82.157.34.130:9901/app/office/extend/use?id=${id}&minute=${minute}`,
					header: {
						'Authorization': getApp().globalData.token,
					},
					success: (res) => {
						console.log(res);
					}
				})

			}
		},
		components: {
			tarbar,
			tarbarHeader,
			environmentItem,
			tip,
			dash,
			doubleButton,
			divView,
			warn,
		},
		computed: {
			getColor() {
				if (this.grade <= 33) {
					return 'linear-gradient(270deg, #FBD66A 0%, #FEB97A 20%, rgba(251,105,108,0.80) 100%)'
				} else if (this.grade > 33 && this.grade <= 66) {
					return 'linear-gradient(90deg, #B4EE6B 0%, #FFE862 100%)'
				} else if (this.grade > 66)
					return 'linear-gradient(90deg, rgba(17,174,174,0.80) 3%, #A8EF69 100%)'


			},
			getLeft() {
				return this.grade + "%";
			},
			getSrc() {
				if (this.grade <= 33) {
					return '../../static/app/btn-quxiao@2X.png'
				} else if (this.grade > 33 && this.grade <= 66) {
					return '	../../static/app/chazuo@2X.png'
				} else if (this.grade > 66)
					return '	../../static/app/btn-yangchang@2X.png'




			},

			getButtonIndex() {

				return this.buttonIndex
			},

		}
	}
</script>

<style>
	.warnning {

		position: absolute;
		top: 0;
		left: 0;
		z-index: 22;
	}

	.warn-show {
		display: block;
	}

	.warn-hidden {
		display: none;
	}

	.bgc {
		overflow: hidden;
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

	.content-top {
		background-image: url(../../static/app/logo@2X.png);
		background-repeat: no-repeat;
		background-position: center calc(100vh * 08/812);
		background-size: calc(750rpx * 48/ 375);

	}

	.content-main .firstShow {
		overflow: hidden;
		width: calc(750rpx * 343/ 375);
		height: calc(100vh * 329/812);
		border-radius: calc(750rpx * 30/ 375);
		margin-left: calc(750rpx * 16/ 375);
		margin-right: calc(750rpx * 16/ 375);
		margin-top: calc(100vh * 10/812);
		background-color: rgba(255, 255, 255, 1);



	}

	.content-main .firstShow .firstShow-top {
		margin-top: calc(100vh * 24/812);
		height: calc(100vh * 45/812);

		display: flex;

	}

	.content-main .firstShow .firstShow-top .left {
		margin-left: calc(750rpx * 24/ 375);
		margin-right: calc(750rpx * 18/ 375);
	}

	.content-main .firstShow .firstShow-top .right {
		display: flex;
		flex-direction: column;
	}

	.content-main .firstShow .firstShow-center .top-box {
		display: flex;
		margin-top: calc(100vh * 24/812);

	}

	.content-main .firstShow .firstShow-center .top-box .item {
		width: calc(750rpx * 16/ 375);
		height: calc(100vh * 16/812);
		margin-left: calc(750rpx * 1/ 375);

	}

	.content-main .firstShow .firstShow-center .top-box .colorOne {
		background-color: #D0F3F3;
	}

	.content-main .firstShow .firstShow-center .top-box .colorTwo {
		background-color: #71DADA;
	}

	.content-main .firstShow .firstShow-center .top-box .colorThree {
		background-color: #FB696C;
	}

	.content-main .firstShow .firstShow-center .top-box .item:nth-child(1) {
		margin-left: calc(750rpx * 19/ 375);
	}

	.content-main .firstShow .firstShow-center .center-box {
		margin-top: calc(100vh * 8/812);
		padding-top: calc(100vh * 8/812);
		color: rgba(10, 32, 57, 0.5);
		margin-left: calc(750rpx * 20/ 375);
		margin-right: calc(750rpx * 19/ 375);
		font-size: calc(750rpx * 10/ 375);
		display: flex;
		justify-content: space-between;
		border-top: calc(750rpx * 1 / 375) dashed rgba(10, 32, 57, 0.15);
	}

	.content-main .firstShow .firstShow-bottom {
		width: calc(750rpx * 304/ 375);
		height: calc(100vh * 53/812);
		margin-left: calc(750rpx * 24/ 375);

		border-bottom: calc(750rpx * 1 / 375) dashed rgba(10, 32, 57, 0.15);
		display: flex;
		align-items: center;
	}

	.content-main .firstShow .firstShow-bottom text {

		position: relative;
	}

	.content-main .firstShow .firstShow-bottom text:nth-child(1) {
		margin-left: calc(750rpx * 84.5/ 375);
	}

	.content-main .firstShow .firstShow-bottom text:nth-child(1)::before {
		content: '';
		display: block;
		width: calc(750rpx * 8/ 375);
		height: calc(750rpx * 8/ 375);
		border-radius: 50%;
		background-color: rgba(19, 194, 194, 1);
		position: absolute;
		top: 50%;
		left: -50%;
		transform: translateY(-50%);




	}

	.content-main .firstShow .firstShow-bottom text:nth-child(2) {
		margin-left: calc(750rpx * 34/ 375);
	}

	.content-main .firstShow .firstShow-bottom text:nth-child(2)::before {
		content: '';
		display: block;
		width: calc(750rpx * 8/ 375);
		height: calc(750rpx * 8/ 375);
		border-radius: 50%;
		background-color: rgba(19, 194, 194, 0.4);
		position: absolute;
		top: 50%;
		left: -50%;
		transform: translateY(-50%);

	}

	.content-main .firstShow .firstShow-bottom text:nth-child(3) {
		margin-left: calc(750rpx * 34/ 375);
	}

	.content-main .firstShow .firstShow-bottom text:nth-child(3)::before {
		content: '';
		display: block;
		width: calc(750rpx * 8/ 375);
		height: calc(750rpx * 8/ 375);
		border-radius: 50%;
		background-color: rgba(251, 105, 108, 1);
		position: absolute;
		top: 50%;
		left: -50%;
		transform: translateY(-50%);

	}

	.content-main .firstShow .firstShow-bottom-bottom {
		width: calc(750rpx * 295/ 375);
		height: calc(750rpx * 131/ 375);

		margin-left: calc(750rpx * 24 / 375);
		;
	}

	.content-main .firstShow .firstShow-bottom-bottom .top-box {
		display: flex;
		justify-content: space-between;
		margin-top: calc(750rpx * 12/ 375);
		align-items: center;

	}

	.content-main .firstShow .firstShow-bottom-bottom .bottom-box .top {
		margin-top: calc(100vh * 47/812);
		display: flex;
	}

	.content-main .firstShow .firstShow-bottom-bottom .top .long {
		width: calc(750rpx * 98.92/ 375);
		height: calc(750rpx * 9.73/ 375);
		background-color: #007AFF;
	}

	.content-main .firstShow .firstShow-bottom-bottom .top .long:nth-child(2) {

		border-top-left-radius: calc(750rpx * 100/ 375);
		border-bottom-left-radius: calc(750rpx * 100/ 375);
		background-image: linear-gradient(270deg, #FBD66A 0%, #FEB97A 20%, rgba(251, 105, 108, 0.80) 100%);
		;
	}

	.content-main .firstShow .firstShow-bottom-bottom .top .long:nth-child(3) {
		margin-left: calc(750rpx * 1/ 375);
		background-image: linear-gradient(90deg, #B4EE6B 0%, #FFE862 100%);
		;
	}

	.content-main .firstShow .firstShow-bottom-bottom .top .long:nth-child(4) {
		border-top-right-radius: calc(750rpx * 100/ 375);
		border-bottom-right-radius: calc(750rpx * 100/ 375);
		margin-left: calc(750rpx * 1/ 375);
		background-image: linear-gradient(90deg, rgba(17, 174, 174, 0.80) 3%, #A8EF69 100%);
	}

	.content-main .secondShow {
		overflow: hidden;
		width: calc(750rpx * 343/ 375);
		height: calc(100vh * 246/812);
		border-radius: calc(750rpx * 30/ 375);
		margin-left: calc(750rpx * 16/ 375);
		margin-right: calc(750rpx * 16/ 375);
		margin-top: calc(100vh * 21/812);
		box-shadow: 0 calc(750rpx * 2/ 375) calc(100vh * 10/812) 0 rgba(10, 32, 57, 0.08);
		background-color: rgba(255, 255, 255, 1);
	}

	.content-main .secondShow .secondShow-box {
		width: 100%;
		height: calc(100vh * 194/812);
		/* margin:calc(100vh * 20/812) calc(750rpx * 18/ 375) 0 calc(750rpx * 18/ 375); */

	}

	.content-main .secondShow text {
		color: rgba(10, 32, 57, 0.5);
		width: calc(750rpx * 48/ 375);
		height: calc(100vh * 24/812);
		line-height: calc(100vh * 24/812);
		border-radius: calc(750rpx * 16/ 375);
		display: inline-block;
		text-align: center;

	}

	.content-main .secondShow text:nth-child(2) {
		margin-left: calc(750rpx * 12/ 375);
		margin-right: 0;
	}

	.content-main .secondShow .click {
		color: white;
		background-color: #13C2C2;
	}

	.content-main .safeFirstShow {
		overflow: hidden;
		width: calc(750rpx * 343/ 375);
		height: calc(100vh * 281/812);
		border-radius: calc(750rpx * 30/ 375);
		margin-left: calc(750rpx * 16/ 375);
		margin-right: calc(750rpx * 16/ 375);
		margin-top: calc(100vh * 10/812);
		background-color: rgba(255, 255, 255, 1);
	}

	.content-main .safeSecondShow-box,
	.safeFirstShow-box {
		width: 100%;
		height: calc(100vh * 194/812);

	}

	.content-main .safeSecondShow {
		overflow: hidden;
		width: calc(750rpx * 343/ 375);
		height: calc(100vh * 281/812);
		border-radius: calc(750rpx * 30/ 375);
		margin-left: calc(750rpx * 16/ 375);
		margin-right: calc(750rpx * 16/ 375);
		margin-top: calc(100vh * 21/812);
		box-shadow: 0 calc(750rpx * 2/ 375) calc(100vh * 10/812) 0 rgba(10, 32, 57, 0.08);
		background-color: rgba(255, 255, 255, 1);
	}

	.content-main .safeSecondShow text {
		color: rgba(10, 32, 57, 0.5);
		width: calc(750rpx * 48/ 375);
		height: calc(100vh * 24/812);
		line-height: calc(100vh * 24/812);
		border-radius: calc(750rpx * 16/ 375);
		display: inline-block;
		text-align: center;
	}

	.content-main .safeSecondShow text:nth-child(2) {
		margin-left: calc(750rpx * 12/ 375);
		margin-right: 0;
	}

	.content-main .safeSecondShow .click {
		color: white;
		background-color: #13C2C2;
	}

	.content-main .firstOffice {
		overflow: hidden;
		height: calc(100vh * 416/812);
		width: calc(750rpx * 343/ 375);
		border-radius: calc(750rpx * 30/ 375);
		margin-left: calc(750rpx * 16/ 375);
		margin-right: calc(750rpx * 16/ 375);
		margin-top: calc(100vh * 10/812);
		background-color: rgba(255, 255, 255, 1);
	}

	.content-main .firstOffice .environment {
		display: flex;
		flex-wrap: wrap;

	}

	.content-main .firstOffice .environment .environment-item:nth-child(1) {
		margin-left: calc(750rpx *24/ 375);
		margin-top: calc(100vh * 20/812);
	}

	.content-main .firstOffice .environment .environment-item:nth-child(2) {
		margin-top: calc(100vh * 20/812);
		margin-left: calc(750rpx * 12/ 375)
	}

	.content-main .firstOffice .environment .environment-item:nth-child(3) {
		margin-top: calc(100vh * 20/812);
		margin-left: calc(750rpx * 12/ 375)
	}

	.content-main .firstOffice .environment .environment-item:nth-child(4) {
		margin-left: calc(750rpx *24/ 375);
		margin-top: calc(100vh * 10/812);
	}

	.content-main .firstOffice .environment .environment-item:nth-child(5) {
		margin-left: calc(750rpx * 12/ 375);
		margin-top: calc(100vh * 10/812);
	}

	.content-main .firstOffice .environment .environment-item:nth-child(6) {
		margin-left: calc(750rpx * 12/ 375);
		margin-top: calc(100vh * 10/812);
	}


	.content-main .firstOffice .firstOffice-tip {
		font-size: calc(750rpx * 12/ 375);
		text-align: center;
		margin-top: calc(750rpx * 12.5/ 375);
		color: rgba(10, 32, 57, 0.5);
	}

	.content-main .firstOffice .firstOffice-tip::before {
		content: '';
		display: inline-block;
		width: calc(750rpx * 11/ 375);
		height: calc(100vh* 12/ 812);

		margin-right: calc(750rpx * 9.93/ 375);
		background-size: cover;

	}

	.content-main .secondOffice {
		overflow: hidden;
		width: calc(750rpx * 343/ 375);
		height: calc(100vh * 168/812);
		border-radius: calc(750rpx * 30/ 375);
		margin-left: calc(750rpx * 16/ 375);
		margin-right: calc(750rpx * 16/ 375);
		margin-top: calc(100vh * 21/812);
		box-shadow: 0 calc(750rpx * 2/ 375) calc(100vh * 10/812) 0 rgba(10, 32, 57, 0.08);
		background-color: rgba(255, 255, 255, 1);

	}

	.content-main .secondOffice .secondOffice-item {
		display: flex;
		justify-content: space-between;
		color: rgba(10, 32, 57, 0.7);
		font-size: calc(750rpx * 12/ 375);
	}

	.content-main .reserveFirst {
		overflow: hidden;
		width: calc(750rpx * 343/ 375);
		height: calc(100vh * 100/812);
		border-radius: calc(750rpx * 30/ 375);
		margin-left: calc(750rpx * 16/ 375);
		margin-right: calc(750rpx * 16/ 375);
		margin-top: calc(100vh * 16/812);
		box-shadow: 0 calc(750rpx * 2/ 375) calc(100vh * 10/812) 0 rgba(10, 32, 57, 0.08);
		background-color: rgba(255, 255, 255, 1);
		display: flex;
		justify-content: space-around;
		align-items: center;
	}

	.content-main .reserveFirst .reserveFirst-left {
		width: calc(750rpx * 68/ 375);
		height: calc(100vh * 70/812);
		background-size: contain;
		background-repeat: no-repeat;
		margin-left: calc(-750rpx * 21/ 375);
	}

	.content-main .reserveFirst .reserveFirst-center {
		margin-left: calc(-750rpx * 68/ 375);
		background-size: cover;

	}

	.content-main .reserveFirst .reserveFirst-center text {
		font-size: calc(750rpx * 14/ 375);
		display: block;
		width: auto;
		height: calc(100vh * 20/812);
	}

	.content-main .reserveFirst .reserveFirst-center text:first-child {
		color: rgba(10, 32, 57, 0.5);
	}

	.content-main .reserveFirst .reserveFirst-center text:last-child {
		color: rgba(10, 32, 57, 0.9);
		margin-top: calc(100vh *8/812);
	}

	.content-main .reserveFirst .reserveFirst-right {
		font-size: calc(750rpx * 14/ 375);
		color: rgba(255, 255, 255, 1);
		background-color: #13C2C2;
		width: calc(750rpx * 48/ 375);
		height: calc(100vh * 24/812);
		line-height: calc(100vh * 24/812);
		border-radius: calc(750rpx * 15.5/ 375);
		text-align: center;
		position: relative;
	}

	.content-main .reserveFirst .reserveFirst-right .close {
		visibility: hidden;
	}

	.content-main .reserveFirst .reserveFirst-right .open {
		visibility: hidden;
	}

	.content-main .reserveFirst .reserveFirst-right-click {
		background-color: #636e72;
	}

	.content-main .reserveFirst .reserveFirst-right .reserveFirst-right-radius {
		position: absolute;
		top: 10%;
		left: 5%;
		width: calc(750rpx * 20/ 375);
		height: calc(100vh * 20/812);
		background-color: #F8F8F8;
		border-radius: 50%;
	}

	.content-main .reserveFirst .reserveFirst-right .reserveFirst-right-radius-click {
		position: absolute;
		top: 10%;
		left: 55%;
		bottom: 0;
		background-color: #F8F8F8;

	}

	.content-main .show-first {
		position: relative;
	}

	.content-main .show-first .reserveText {
		height: calc(100vh * 45/812);
		width: calc(750rpx * 91/ 375);
		line-height: calc(100vh * 45/812);
		text-align: center;
		margin: calc(100vh * 40/812) calc(750rpx * 142/ 375) 0;
		font-size: calc(750rpx * 32/ 375);
		color: rgba(255, 255, 255, 1);

	}

	.content-main .show-first .reserveText::before {

		content: '固定工位 :';
		display: block;
		font-size: calc(750rpx * 14/ 375);
		height: calc(100vh *20/812);
		width: calc(750rpx *70/ 375);
		position: absolute;
		top: calc(-100vh *40/812);
		left: 50%;
		transform: translateX(-50%)
	}

	.content-main .reserveSecond {
		overflow: hidden;
		width: calc(750rpx * 343/ 375);
		height: calc(100vh * 297/812);
		border-radius: calc(750rpx * 30/ 375);
		margin-left: calc(750rpx * 16/ 375);
		margin-right: calc(750rpx * 16/ 375);
		margin-top: calc(100vh * 21/812);
		box-shadow: 0 calc(750rpx * 2/ 375) calc(100vh * 10/812) 0 rgba(10, 32, 57, 0.08);
		background-color: rgba(255, 255, 255, 1);

	}

	.content-main .reserveSecond .reserveSecond-box {
		width: 100%;
		height: calc(100vh * 216/812);

	}

	.content-main .reserveSecond .reserveSecond-img {
		width: calc(750rpx * 295/ 375);
		height: calc(100vh * 160/812);

		margin: calc(100vh * 15/812) calc(750rpx * 26/ 375) calc(750rpx * 22/ 375);
		padding-top: calc(100vh * 15/812);
		border-top: 1px dashed #808080;
		text-align: center;

	}



	.content-main .reserveSecond .right {
		display: flex;
		color: #FFA936;
		font-size: calc(750rpx * 13/ 375);

	}

	.content-main .reserveSecond .reserveSecond-box {}

	.content-main .show-second .reserveFirst {
		margin-top: calc(100vh * 20/812);
		background-color: rgba(10, 32, 57, 0.1);
		display: flex;
	}

	.content-main .show-second .reserveFirst .left text {
		display: block;
		color: rgba(255, 255, 255, 0.65);
	}

	.content-main .show-second .reserveFirst .left text:nth-child(1) {
		font-size: calc(750rpx * 24/ 375);
		color: rgba(255, 255, 255, 1);
	}

	.content-main .show-second .reserveFirst .left text:nth-child(2) {
		margin-top: calc(100vh * 8/812);
	}

	.content-main .show-second .reserveFirst .right image {
		width: calc(750rpx * 60/ 375);
		height: calc(100vh * 60/812);
	}

	.content-main .show-second .afterSign .picker {
		z-index: 1;
		left: 0;
		bottom: 0;
	}

	.content-main .show-second .afterSign .picker .picker-item {

		background-color: white;
		text-align: center;
	}
</style>
