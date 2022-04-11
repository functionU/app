<template>
	<view>
		<view class="bgc">
			<view class="content">
				<view class="content-top">
					<tarbarHeader class="head" @newsClick="newsClickListen" :showObj="showMessage" v-if="headerShow"
						@scanClick="scanClickListen">
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
									<text
										style="font-size: calc(750rpx * 16/ 375);font-weight: bold;">您已连续工作{{continueWorkTimeHour}}小时{{continueWorkTimeMins}}分钟！</text>
									<text
										style="font-size: calc(750rpx * 12/ 375); color: #666D7F;margin-top:calc(100vh * 6/812) ;">在工位上的时间：{{ workTimeHour}}小时{{workTimeMins}}分钟</text>
								</view>
							</view>
							<view class="firstShow-center">
								<view
									style="width: calc(750rpx * 305	/ 375);margin-left: calc(750rpx * 19/ 375);overflow-x: scroll;">
									<view id="top-box" ref="topBox" style="margin-left: 0;">
										<view class="item" v-for="item in boxArray"
											:class="{'colorOne':item==0,'colorTwo':item==1,'colorThree':item==2,'colorFour':item==4}">
										</view>
									</view>
									<view class="center-box" ref='centerBox' style="margin-left: 0;"
										:style="{'width':width+'rpx'}">
										<view>
											<image src="../../static/app/toWork.svg"
												style="width: calc(750rpx * 10/ 375);height: calc(750rpx * 9.8/ 375); margin-right: calc(750rpx * 4/ 375) ;">
											</image>
											{{timelist[0]}}
										</view>
										<view>
											{{timelist[1]}}
										</view>
										<view>
											{{timelist[2]}}
										</view>
										<view>
											{{timelist[3]}}
											<image src="../../static/app/toRest.svg"
												style="width: calc(750rpx * 10/ 375);height: calc(750rpx * 9.8/ 375); margin-left: calc(750rpx * 4/ 375)">
											</image>
										</view>
									</view>
								</view>
								<!-- 	<view id="top-box" ref="topBox">
									<view class="item" v-for="item in boxArray"
										:class="{'colorOne':item==0,'colorTwo':item==1,'colorThree':item==2,'colorFour':item==4}">
									</view>
								</view>
								<view class="center-box" ref='centerBox'>
									<view>
										<image src="../../static/app/toWork.svg"
											style="width: calc(750rpx * 10/ 375);height: calc(750rpx * 9.8/ 375); margin-right: calc(750rpx * 4/ 375) ;">
										</image>
										{{timelist[0]}}
									</view>
									<view>
										{{timelist[1]}}
									</view>
									<view>
										{{timelist[2]}}
									</view>
									<view>
										{{timelist[3]}}
										<image src="../../static/app/toRest.svg"
											style="width: calc(750rpx * 10/ 375);height: calc(750rpx * 9.8/ 375); margin-left: calc(750rpx * 4/ 375)">
										</image>
									</view>
								</view> -->
							</view>

							<view class="firstShow-bottom" style="font-size: calc(750rpx * 12/ 375);">
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
										<view class="radius" :style="{'position':'absolute','left':getLeft,'top':'-50%','background-image':getColor,'border-radius':'50%','transform':
											'translateX(-50%)'}" style="width: calc(750rpx * 18/ 375) ;height:calc(750rpx * 18/ 375);background-color: white;padding:calc(750rpx * 4/ 375) ;box-sizing: border-box;">
											<view
												style="width: 100%;height: 100%; border-radius: 50%;background-color:white;">
												<view id="messageTwo"
													style="position: absolute;top: -190%;left: -50%;width:calc(750rpx * 40/ 375);height:calc(750rpx * 35/375);line-height:calc(750rpx * 40	/375);text-align: center;">
													<image :src="getSrc"
														style="width:calc(750rpx * 20/ 375);height:calc(750rpx * 20/375);">
													</image>
												</view>

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
										style="display: flex; justify-content: space-between;font-size:calc(750rpx * 12/ 375);margin-top:calc(750rpx * 6/375;color: rgba(10, 32, 57, 0.7);">
										<text>久坐</text>
										<text>健康</text>
									</view>
								</view>
							</view>
						</view>
						<view class="secondShow">
							<tip :item="{'name':'久坐统计（分钟）'}">
								<view slot='right' class="button">
									<text @click="weekClick" :class="{'click':secondShowButtonIndex=='week'}">按周</text>
									<text @click="monthClick"
										:class="{'click':secondShowButtonIndex=='month'}">按月</text>
								</view>
							</tip>
							<view class="secondShow-box"
								style="margin-top:calc(100vh * 10/812);margin-left:calc(750rpx * -5/ 375);">
								<qiun-data-charts type="line" :chartData="chartData" :errorShow="false"
									background="none" :reshow="index==0" />
							</view>

						</view>
					</view>
					<view class="content-main" v-show="index==2">
						<view class="safeFirstShow">
							<tip :item="{name:'今日用电（w）'}">
								<text slot="right"></text>
							</tip>
							<view class="safeFirstShow-box" style="margin-top:calc(100vh * 10/812);">
								<qiun-data-charts type="line" :chartData="SafeTodayChartData" :errorShow="false"
									background="none" :reshow="index==2" />
							</view>
						</view>
						<view class="safeSecondShow">
							<tip :item="{name:'用电统计（kwh）'}">
								<view slot='right' class="SafeButton">
									<text @click="safeWeekClick"
										:class="{'click':secondShowSafeButtonIndex=='week'}">按周</text>
									<text @click="safeMonthClick"
										:class="{'click':secondShowSafeButtonIndex=='month'}">按月</text>
								</view>
							</tip>
							<view class="safeSecondShow-box" style="margin-top:calc(100vh * 10/812);">
								<qiun-data-charts type="line" :chartData="SafeChartData" :errorShow="false"
									background="none" :reshow="index==2" />
							</view>

						</view>
					</view>
					<view class="content-main" v-show="index==1">
						<doubleButton :number='getButtonIndex' @doubleButtonClick='doubleButtonListen'></doubleButton>
						<view class="show-first" v-show="buttonIndex==0">
							<view class="reserveText" v-show="fixedTag">
								{{fixedObj.station_number}}
							</view>
							<view class="reserveFirst" v-show="fixedTag">
								<view class="reserveFirst-left bgcOne">

								</view>
								<view class="reserveFirst-center">
									<text>{{fixedItem.english}}</text>
									<text>{{fixedItem.chinese}}</text>
								</view>

								<view class="reserveFirst-right" @click="powerFixedButtonClick(fixedObj.device_id)"
									:class="{'reserveFirst-right-click':powerFixedButton}">
									<view
										:class="{'reserveFirst-right-radius':true,'reserveFirst-right-radius-click':powerFixedButton}">
									</view>
									<text :class="{'close':!powerFixedButton}">关</text>
									<text style="visibility: hidden;">1</text>
									<text :class="{'open':powerFixedButton}">开</text>
								</view>

							</view>
							<view v-show="!fixedTag"
								style="text-align: center;margin-top: 20%;color: white;font-weight: bold;">
								您还没有固定工位呢！
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
								<view
									style="height: calc(100vh * 194/812);width: 100%;margin-top:calc(100vh * 20/812);;">
									<qiun-data-charts type="column" :chartData="reserveChartData" background="none"
										:reshow="index==1&&buttonIndex==0" />

								</view>
							</view>

						</view>
						<view class="show-second" v-show="buttonIndex==1">
							<view v-show="!resever">
								<view class="reserveFirst">

									<view class="left">
										<text
											style="font-weight: Semibold;font-size: calc(750rpx * 21/ 375);">快速预约</text>
										<text>选择工位和时间进行使用～</text>
									</view>
									<view class="right" @click="reserveClick">
										<image src="../../static/app/reserve.svg"></image>
									</view>
								</view>
								<view
									style="height: calc(100vh * 467/812);overflow-y:scroll;margin-top:  calc(100vh * 20/812);border-radius: calc(750rpx * 30/ 375);">

									<view style="overflow-x: scroll;">
										<view v-show="infoTag" v-for="(listObj,index) in list"
											style="height: calc(100vh * 169/812);background-color: white;margin-left:calc(750rpx * 16/ 375);margin-right: calc(750rpx * 16/ 375);overflow: hidden; margin-top:calc(100vh * 20/812);border-radius:calc(750rpx * 30 / 375);">
											<view>
												<tip :item="{name:'我的预约',}">
													<text slot='right'
														style="font-size:calc(750rpx * 24/ 375) ;color: black;margin-top: calc(100vh * 23/812);margin-bottom: calc(100vh * 21/812);">{{listObj.station_number}}</text>
												</tip>
												<dash style="margin-top: calc(750rpx * 10/ 375);"></dash>
												<view
													style="margin-top: calc(750rpx * 11/ 375);margin-left:calc(750rpx * 29/ 375);">
													<image src="../../static/app/usePosition.svg"
														style="width:calc(750rpx * 14/ 375);height:calc(750rpx * 14/ 375);margin-right:calc(750rpx * 8/ 375);">
													</image>
													<text>{{listObj.office_building_name}}-{{listObj.floor_name}}</text>
												</view>

											</view>
											<view
												style="margin-top: calc(750rpx * 12/ 375);margin-left:calc(750rpx * 29/ 375);">
												<image src="../../static/app/useTime.svg"
													style="width:calc(750rpx * 14/ 375);height:calc(750rpx *14/ 375);margin-right:calc(750rpx * 8/ 375);">
												</image>
												<text>{{listObj.reserve_date}} {{listObj.start_time.split(":")[0]}}:{{listObj.start_time.split(":")[1]}}~{{listObj.end_time.split(":")[0]}}:{{listObj.end_time.split(":")[1]}}</text>
												
											</view>
											<view @click="stopInfo(listObj.id,index) "
												style="font-size: 25rpx;text-align: center;margin-top:  calc(100vh * 8/812);border-radius:calc(750rpx * 30/ 375);width: calc(750rpx * 88/ 375);height: calc(100vh * 25/812);margin-left: 35%;border:calc(750rpx * 1/ 375)  solid  #1ABFC2;line-height:  calc(100vh * 25/812);color: #1ABFC2;">
												取消预约
											</view>
										</view>
										<view
											style="display: flex;flex-direction: column;background-color: white;height: calc(100vh * 315	/812);margin-left:calc(750rpx * 16/ 375);margin-right: calc(750rpx * 16/ 375);border-radius: 30rpx;"
											v-if="!infoTag">
											<tip :item="{name:'我的预约'}" style="margin-bottom:  calc(100vh * 21/812);">
												<text slot='right'
													style="font-size:calc(750rpx * 24/ 375) ;color: black;"></text>
											</tip>
											<dash></dash>
											<image src="../../static/app/nodata.svg"
												style="width: calc(750rpx * 120/ 375);height: calc(100vh * 120/812);margin-left: calc(750rpx * 112 / 375);margin-top:  calc(100vh * 15/812);">
												<text
													style="color: rgba(10, 32, 57, 0.5);margin-top:  calc(100vh * 17/812);margin-left: calc(750rpx * 108 / 375);"
													v-if="!infoTag">还没有预订工位～</text>
												<text
													style="color: rgba(26, 191, 194, 1);margin-left: 40%;margin-top: calc(100vh * 6/812);"
													@click="reserveClick">点击预订</text>
										</view>



									</view>


								</view>
							</view>
							<view v-show="resever">
								<view class="reserveFirst" style="height: calc(100vh * 83/812);">
									<view class="right">
										<image src="../../static/app/position.svg"></image>
									</view>
									<view class="left">
										<text>{{infoObj.station_number}}</text>
										<text>{{infoObj.start_time}}～{{infoObj.end_time}}</text>
										
										
									</view>

								</view>
								<view class="beforeSign" v-show="!sign">
									<view
										style="	width: calc(750rpx * 343/ 375);height: calc(100vh * 148/812);display: flex; margin-top: calc(100vh * 20/812);">
										<view @click="signIn(infoObj.id)"
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
									<view
										style="height: calc(100vh * 287/812);overflow-y:scroll;margin-top:  calc(100vh * 0/812);border-radius: calc(750rpx * 30/ 375);">

										<view style="overflow-x: scroll;">
											<view v-show="infoTag" v-for="(listObj,index) in list"
												style="height: calc(100vh * 169/812);background-color: white;margin-left:calc(750rpx * 16/ 375);margin-right: calc(750rpx * 16/ 375);overflow: hidden; margin-top:calc(100vh * 20/812);border-radius:calc(750rpx * 30 / 375);">
												<view>
													<tip :item="{name:'我的预约',}">
														<text slot='right'
															style="font-size:calc(750rpx * 24/ 375) ;color: black;margin-top: calc(100vh * 23/812);margin-bottom: calc(100vh * 21/812);">{{listObj.station_number}}</text>
													</tip>
													<dash style="margin-top: calc(750rpx * 10/ 375);"></dash>
													<view
														style="margin-top: calc(750rpx * 11/ 375);margin-left:calc(750rpx * 29/ 375);">
														<image src="../../static/app/usePosition.svg"
															style="width:calc(750rpx * 14/ 375);height:calc(750rpx * 14/ 375);margin-right:calc(750rpx * 8/ 375);">
														</image>
														<text>{{listObj.office_building_name}}-{{listObj.floor_name}}</text>
													</view>

												</view>
												<view
													style="margin-top: calc(750rpx * 12/ 375);margin-left:calc(750rpx * 29/ 375);">
													<image src="../../static/app/useTime.svg"
														style="width:calc(750rpx * 14/ 375);height:calc(750rpx *14/ 375);margin-right:calc(750rpx * 8/ 375);">
													</image>
													<text>{{listObj.reserve_date}} {{listObj.start_time.split(":")[0]}}:{{listObj.start_time.split(":")[1]}}~{{listObj.end_time.split(":")[0]}}:{{listObj.end_time.split(":")[1]}}</text>
												</view>
												<view @click="stopInfo(listObj.id,index) "
													style="font-size: 25rpx;text-align: center;margin-top:  calc(100vh * 8/812);border-radius:calc(750rpx * 30/ 375);width: calc(750rpx * 88/ 375);height: calc(100vh * 25/812);margin-left: 35%;border:calc(750rpx * 1/ 375)  solid  #1ABFC2;line-height:  calc(100vh * 25/812);color: #1ABFC2;">
													取消预约
												</view>
											</view>
											<view
												style="display: flex;flex-direction: column;background-color: white;height: calc(100vh * 267/812);margin-left:calc(750rpx * 16/ 375);margin-right: calc(750rpx * 16/ 375);border-radius: 30rpx;margin-top: calc(100vh * 20/812);"
												v-if="!infoTag">
												<tip :item="{name:'我的预约'}"
													style="margin-bottom:  calc(100vh * 21/812);">
													<text slot='right'
														style="font-size:calc(750rpx * 24/ 375) ;color: black;"></text>
												</tip>
												<dash></dash>
												<image src="../../static/app/nodata.svg"
													style="width: calc(750rpx * 120/ 375);height: calc(100vh * 120/812); margin-left: calc(750rpx * 112 / 375);	">
													<text
														style="color: rgba(10, 32, 57, 0.5);margin-top:  calc(100vh * 17/812);margin-left: calc(750rpx * 108 / 375);"
														v-if="!infoTag">还没有预订工位～</text>
													<text
														style="color: rgba(26, 191, 194, 1);margin-left: 40%;margin-top: calc(100vh * 6/812);"
														@click="reserveClick">点击预订</text>
											</view>



										</view>


									</view>
								</view>
								<view class="afterSign" v-show="sign">
									<view class="reserveFirst" style="background-color: white;">
										<view class="reserveFirst-left bgcOne">

										</view>
										<view class="reserveFirst-center ">
											<text>{{item.english}}</text>
											<text>{{item.chinese}}</text>
										</view>

										<view class="reserveFirst-right" @click="powerButtonClick(infoObj.device_id)"
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

										<view class="picker" v-if="addUse"
											style="position: fixed;bottom: 0;left: 0;right: 0;">
											<view
												style="display:flex;justify-content: space-between;background-color: white;border-top-left-radius:30rpx ;border-top-right-radius:30rpx;">
												<text @click="addCancal"
													style="font-size: calc(750rpx * 14/ 375);padding: calc(750rpx * 9/ 375);margin-top: calc(100vh * 10/812);font-weight: bold;margin-left:calc(750rpx * 4/ 375);">取消</text>
												<text
													style="font-weight: bold;margin-top: calc(100vh * 16/812);">延长使用</text>
												<text @click="addSubmit"
													style="font-size: calc(750rpx * 14/ 375);padding: calc(750rpx * 9 /375) ;color: rgba(19, 194, 194, 1);margin-top: calc(100vh * 10/812);font-weight: bold;margin-right: calc(750rpx * 4/ 375);">确定</text>
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

									<view
										style="height: calc(100vh * 267/812);overflow-y:scroll;border-radius: calc(750rpx * 30/ 375);">

										<view style="overflow-x: scroll;">
											<view v-show="infoTag" v-for="(listObj,index) in list"
												style="height: calc(100vh * 169/812);background-color: white;margin-left:calc(750rpx * 16/ 375);margin-right: calc(750rpx * 16/ 375);overflow: hidden; margin-top:calc(100vh * 20/812);border-radius:calc(750rpx * 30 / 375);">
												<view>
													<tip :item="{name:'我的预约',}">
														<text slot='right'
															style="font-size:calc(750rpx * 24/ 375) ;color: black;margin-top: calc(100vh * 23/812);margin-bottom: calc(100vh * 21/812);">{{listObj.station_number}}</text>
													</tip>
													<dash style="margin-top: calc(750rpx * 10/ 375);"></dash>
													<view
														style="margin-top: calc(750rpx * 11/ 375);margin-left:calc(750rpx * 29/ 375);">
														<image src="../../static/app/usePosition.svg"
															style="width:calc(750rpx * 14/ 375);height:calc(750rpx * 14/ 375);margin-right:calc(750rpx * 8/ 375);">
														</image>
														<text>{{listObj.office_building_name}}-{{listObj.floor_name}}</text>
													</view>

												</view>
												<view
													style="margin-top: calc(750rpx * 12/ 375);margin-left:calc(750rpx * 29/ 375);">
													<image src="../../static/app/useTime.svg"
														style="width:calc(750rpx * 14/ 375);height:calc(750rpx *14/ 375);margin-right:calc(750rpx * 8/ 375);">
													</image>
													<text>{{listObj.reserve_date}} {{listObj.start_time.split(":")[0]}}:{{listObj.start_time.split(":")[1]}}~{{listObj.end_time.split(":")[0]}}:{{listObj.end_time.split(":")[1]}}</text>
												</view> 
												<view @click="stopInfo(listObj.id,index) "
													style="font-size: 25rpx;text-align: center;margin-top:  calc(100vh * 8/812);border-radius:calc(750rpx * 30/ 375);width: calc(750rpx * 88/ 375);height: calc(100vh * 25/812);margin-left: 35%;border:calc(750rpx * 1/ 375)  solid  #1ABFC2;line-height:  calc(100vh * 25/812);color: #1ABFC2;">
													取消预约
												</view>
											</view>
											<view
												style="display: flex;flex-direction: column;background-color: white;height: calc(100vh * 180/812);margin-left:calc(750rpx * 16/ 375);margin-right: calc(750rpx * 16/ 375); overflow: hidden;margin-top:calc(100vh * 18/812);border-radius: 30rpx;"
												v-if="!infoTag">
												<tip :item="{name:'我的预约'}"
													style="margin-bottom:  calc(100vh * 21/812);">
													<text slot='right'
														style="font-size:calc(750rpx * 24/ 375) ;color: black;"></text>
												</tip>
												<image src="../../static/app/nodata.svg"
													style="width: calc(750rpx * 100/ 375);height: calc(100vh * 100/812);margin-left: calc(750rpx * 125 / 375);">
													<text
														style="color: rgba(10, 32, 57, 0.5);margin-top:  calc(100vh * 17/812);margin-left: calc(750rpx * 108 / 375);"
														v-if="!infoTag">还没有预订工位～</text>
													<text
														style="color: rgba(26, 191, 194, 1);margin-left: 40%;margin-top: calc(100vh * 6/812);"
														@click="reserveClick">点击预订</text>
											</view>



										</view>


									</view>
								</view>
							</view>

						</view>

					</view>
					<view class="content-main" v-show="index==3">
						<view class="firstOffice">
							<tip :item="{name:'环境指数',place:'室内传感器'}"></tip>
							<view class="environment">
								<environmentItem v-for="(item,index) in environmentes" :item='item'
									class="environment-item">
								</environmentItem>

							</view>
							<view class="firstOffice-tip">

								建议配备智能硬件，更准确的知晓身边的工作环境～
							</view>
						</view>
						<view class="secondOffice">
							<tip :item="{name:'天气预报',place:place}"></tip>
							<dash style="margin-top: calc(750rpx * 10/ 375);"></dash>
							<view class="box">
								<view class="secondOffice-item" style="margin-left:  calc(750rpx * 24/ 375);"
									v-for="item in weatherBox">
									<view>
										<image style="width: calc(750rpx * 12/ 375);height: calc(100vh * 12/812);"
											:src="getWeatherSrc(item.type)"></image>
										<text
											style="margin-left:  calc(750rpx * 13/ 375);">{{item.week}}-{{item.type}}</text>
									</view>
									<text
										style="margin-right:  calc(750rpx * 24/ 375);">{{item.high.split(" ")[1]}}/{{item.low.split(" ")[1]}}</text>
								</view>
							</view>
						</view>

					</view>
				</view>
			</view>
		</view>
		<warn class="warnning" @confirmClick="confirmListen" :class="{'warn-hidden':!warnShow,'warn-show':warnShow}">
		</warn>
		<view
			style="position: absolute;top: 0;background-color: rgba(0, 0, 0, 0.5);height: 100vh;width: 100%;display: flex;justify-content: center;align-items: center;"
			v-show="reserveModal">
			<view
				style="height: calc(100vh * 118/812);width:calc(750rpx * 311/375);background-color: white;border-radius: 30rpx;display: flex;flex-direction: column;">
				<text
					style="height: calc(100vh * 70/812);text-align: center;line-height: calc(100vh * 70/812);">确定要取消预订的工位么？</text>
				<view style="height: calc(100vh * 48/812) ;display: flex;">
					<view
						style="width: calc(750rpx * 156/375);text-align: center;border-top: 1px solid #dfe6e9;line-height:calc(100vh * 48/812);"
						@click="reserveStopCancal">再想想</view>
					<view
						style="width: calc(750rpx * 156/375);text-align: center;border-left: 1px solid #dfe6e9;border-top: 1px solid #dfe6e9;line-height:calc(100vh * 48/812) ;color: #1ABFC2;"
						@click="reserveStopConfirm">确定取消</view>
				</view>
			</view>
		</view>
		<view
			style="position: absolute;top: 0;background-color: rgba(0, 0, 0, 0.5);height: 100vh;width: 100%;display: flex;justify-content: center;align-items: center;"
			v-show="useStopModal">
			<view
				style="height: calc(100vh * 118/812);width:calc(750rpx * 311/375);background-color: white;border-radius: 30rpx;display: flex;flex-direction: column;font-weight: Regular;font-size: calc(750rpx * 16/375);">
				<text
					style="height: calc(100vh * 70/812);text-align: center;line-height: calc(100vh * 70/812);">您确定要提前结束吗？</text>
				<view style="height: calc(100vh * 48/812) ;display: flex;">
					<view
						style="width: calc(750rpx * 156/375);text-align: center;border-top: 1px solid #dfe6e9;line-height:calc(100vh * 48/812) ;"
						@click="useStopCancel">再想想</view>
					<view
						style="width: calc(750rpx * 156/375);text-align: center;border-left: 1px solid #dfe6e9;border-top: 1px solid #dfe6e9;line-height:calc(100vh * 48/812) ;color: #1ABFC2;"
						@click="useStopConfirm">确定取消</view>
				</view>
			</view>
		</view>
		<view
			style="position: absolute;top: 0;background-color: rgba(0, 0, 0, 0.5);height: 100vh;width: 100%;display: flex;justify-content: center;align-items: center;"
			v-show="stopInfoShow">
			<view
				style="height: calc(100vh * 118/812);width:calc(750rpx * 311/375);background-color: white;border-radius: 30rpx;display: flex;flex-direction: column;font-weight: Regular;font-size: calc(750rpx * 16/375);">
				<text
					style="height: calc(100vh * 70/812);text-align: center;line-height: calc(100vh * 70/812);">您确定要提前结束吗？</text>
				<view style="height: calc(100vh * 48/812) ;display: flex;">
					<view
						style="width: calc(750rpx * 156/375);text-align: center;border-top: 1px solid #dfe6e9;line-height:calc(100vh * 48/812) ;"
						@click="stopInfocancel">再想想</view>
					<view
						style="width: calc(750rpx * 156/375);text-align: center;border-left: 1px solid #dfe6e9;border-top: 1px solid #dfe6e9;line-height:calc(100vh * 48/812) ;color: #1ABFC2;"
						@click="stopInfoConfrim">确定取消</view>
				</view>
			</view>
		</view>
	</view>





</template>
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
				warnShow: false,
				secondShowButtonIndex: 'week',
				secondShowSafeButtonIndex: 'week',

				resever: false,
				environmentesSrc: ['/static/app/1.svg', '/static/app/2.svg', '/static/app/3.svg', '/static/app/4.svg',
					'/static/app/5.svg', '/static/app/6.svg'
				],
				environmentes: [{
					name: '湿度',
					src: '../../static/app/1.svg',
					number: '0',
					quality: '无'
				}, {
					name: '温度',
					src: '../../static/app/2.svg',
					number: '0',
					quality: '无'
				}, {
					name: 'PM2.5',
					src: '../../static/app/3.svg',
					number: '0',
					quality: '无'
				}, {
					name: '噪音',
					src: '../../static/app/4.svg',
					number: '0',
					quality: '无'
				}, {
					name: '甲醛',
					src: '../../static/app/5.svg',
					number: '0',
					quality: '无'
				}, {
					name: '二氧化碳',
					src: '../../static/app/6.svg',
					number: '0',
					quality: '无'
				}],
				chartData: {
					categories: ['', '', '', '', '', '', '周一', '', '', '', '', '', '', '周二', '', '', '', '', '', '',
						'周三', '', '', '', '', '', '', '周四', '', '', '', '', '', '', '周五'
					],
					series: [{
						name: '分钟',
						data: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
							0, 0, 0, 0, 0, 0,
						],
						color: '#13C2C2',
					}]
				},
				SafeChartData: {
					categories: ['', '', '', '', '', '', '周一', '', '', '', '', '', '', '周二', '', '', '', '', '', '', '周三',
						'', '', '', '', '', '', '周四', '', '', '', '', '', '', '周五',
					],
					series: [{
						name: '分钟',
						data: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
							0, 0, 0, 0, 0, 0,
						],
						color: '#13C2C2',
					}]
				},
				SafeTodayChartData: {
					categories: ['', '', '', '', '', '', '周一', '', '', '', '', '', '', '周二', '', '', '', '', '', '', '周三',
						'', '', '', '', '', '', '周四', '', '', '', '', '', '', '周五',
					],
					series: [{
						name: '分钟',
						data: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
							0, 0, 0, 0, 0, 0,
						],
						color: '#13C2C2',
					}]
				},
				reserveChartData: {

					categories: ['00:00', '04:00', '09:00', '14:00', '19:00	'],
					series: [{
						name: '分钟',
						data: [0, 0, 0, 0, 0],
						color: '#FFF',
					}]
				},
				boxArray: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],

				color: '#007AFF',
				reserve: false,
				sign: false,
				timeArray: [15, 30, 45, 60, 90, 120],
				addUse: false,
				addIndex: [0],
				e: [0],
				infoObj: {},
				list: [],
				infoTag: false,
				signTag: false,
				item: {
					english: '--',
					chinese: '----',
					src: '../../static/app/chazuo@2X.png'
				},
				fixedItem: {
					english: '--',
					chinese: '----',
					src: '../../static/app/chazuo@2X.png',

				},
				listArr: [],
				fixedTag: true,
				powerButton: false,
				powerFixedButton: false,
				fixedObj: {},
				showMessage: {},
				headerShow: false,
				count: 0,
				workTimeHour: 0,
				workTimeMins: 0,
				continueWorkTimeHour: 0,
				continueWorkTimeMins: 0,
				grade: 73,
				weatherBox: [],
				timelist: ["8:00", '12:00', '15:00', "19:00"],
				place: "---",
				imgArray: ['/static/app/dull.svg', '/static/app/happy.svg', '/static/app/sad.svg', ],
				weatherArray: [],
				width: "",
				reserveModal: false,
				reserveModalId: -1,
				useStopModal: false,
				useStopModalId: -1,
				weatherImgArray: ['/static/app/qingtian.svg', '/static/app/qingtian.svg', '/static/app/duoyun.svg',
					'/static/app/yintian.svg', '/static/app/zhenyu.svg', '/static/app/baoyu.svg',
					'/static/app/dabaoyu.svg', '/static/app/leizhenyujibanyoubingbao.svg', '/static/app/tedabaoyu.svg',
					'/static/app/xiaoyu.svg', '/static/app/dayu.svg', '/static/app/leizhenyu.svg',
					'/static/app/yujiaxue.svg', '/static/app/zhongyu.svg', '/static/app/daxue.svg',
					'/static/app/xiaoxue.svg', '/static/app/zhongxue.svg', '/static/app/zhenxue.svg',
					'/static/app/baoxue.svg', '/static/app/qiangshachenbao.svg', '/static/app/fuchen.svg',
					'/static/app/wu.svg', '/static/app/shachenbao.svg', '/static/app/dongyu.svg',
					'/static/app/yangsha.svg', '/static/app/mai.svg'
				],
				stopInfoShow: false,
				stopInfoIndex: -1,
				stopInfoId: -1,



			}
		},
		onInit() {

		},
		onBackPress(e) {
			if (e.from == 'backbutton') {
				plus.runtime.quit();
			} else {

				return true;

			}

		},
		onLoad(option) {
			let that = this;
			console.log(getApp().globalData.token)
			uni.getStorage({
				key: 'login',
				success(res) {

					console.log(res);
					if (res.data) {
						// uni.showLoading({
						// 	title: '加载中'
						// })
						uni.request({
							url: `http://${getApp().globalData.http}/platform/app/login`,
							// url: 'http://82.157.34.130:9901/platform/app/login',
							method: 'POST',
							data: {
								password: res.data.password,
								username: res.data.user
							},
							header: {
								'Content-Type': 'application/json'
							},
							success: (res) => {
								// uni.hideLoading();
								console.log(res);
								if (res.data.code != 0) {
									uni.switchTab({
										url: '../login-success/login-success',

									})

								}
								getApp().globalData.token = res.data.value.token;
								if (option.index) {
									that.resever = option.resever;
									that.buttonIndex = parseInt(option.buttonIndex);
									that.index = parseInt(option.index);
									that.startTime = option.startTime;
									that.endTime = option.endTime;
									that.position = option.position;
								}

								uni.request({
									url: `http://${getApp().globalData.http}/app/office/reserve/station/list`,
									// url: 'http://82.157.34.130:9901/app/office/reserve/station/list',
									header: {

										'Authorization': getApp().globalData.token,
									},
									success: (res) => {

										if (Array.isArray(res.data.value) && res.data.value
											.length != 0) {
											that.list = res.data.value;
											that.infoTag = true;
											let arr = new Array([res.data.value.length]);
											arr.fill(true);
											that.listArr = arr;
											console.log(that.listArr)
											// that.resever = true;


										}

									}
								})
								uni.request({
									url: `http://${getApp().globalData.http}/app/office/reserve/station/info`,
									// url: 'http://82.157.34.130:9901/app/office/reserve/station/info',
									header: {
										'Authorization': getApp().globalData.token,
									},
									success: (res) => {

										if (res.data.value) {
											that.resever = true;
											that.sign = false;
											res.data.value.start_time=res.data.value.start_time.split(":")[0]+":"+res.data.value.start_time.split(":")[1];
											res.data.value.end_time=res.data.value.end_time.split(":")[0]+":"+res.data.value.end_time.split(":")[1];
											console.log("+++++++")
											console.log(res.data.value.start_time);
											that.infoObj = res.data.value;
											
											that.sign = res.data.value.sign_status;
											that.powerButton = !res.data.value
												.power_status;
											if (that.powerButton == false) {
												that.item.chinese = '电源开启'
												that.item.english = 'On'
											} else {
												that.item.chinese = '电源关闭'
												that.item.english = 'Off'
											}

										}

									}
								})

								uni.request({
									url: `http://${getApp().globalData.http}/app/office/fixed/station/info`,
									// url: 'http://82.157.34.130:9901/app/office/fixed/station/info',
									header: {
										'Authorization': getApp().globalData.token,
									},
									success: (res) => {

										if (!res.data.value) {

											that.fixedTag = false;
										} else {

											that.fixedObj = res.data.value;
											that.powerFixedButton = !res.data.value
												.power_status;
											if (that.powerFixedButton == false) {
												that.fixedItem.chinese = '电源开启'
												that.fixedItem.english = 'On'
											} else {
												that.fixedItem.chinese = '电源关闭'
												that.fixedItem.english = 'Off'
											}
										}
									}
								})
								uni.request({
										url: `http://${getApp().globalData.http}/app/message/not/read/count`,
										// url: 'http://82.157.34.130:9901/app/message/not/read/count',
										header: {
											'Authorization': getApp().globalData.token,
										},
										success: (res) => {
											if (res.data.value > 0) {

												that.showMessage.showMessage = res.data.value;
												that.showMessage.show = true;


											}
											that.count = res.data.value;
											that.headerShow = true;
										}
									}),
									new Promise(function(resolve, reject) {
										uni.request({
											url: `http://${getApp().globalData.http}/app/data/sit/statistics?type=${1}`,

											header: {
												'Authorization': getApp().globalData.token,
											},
											success: (res) => {
												if (res.data.code == 0 && res.data
													.value.length != 0) {
													resolve(res.data.value);
												} else {
													reject();
													uni.showToast({
														title: res.data
															.message,
														icon: 'none',
														duration: 2000
													});
												}

											}
										})
									}).then(res => {
										let x = [];
										let y = [];
										res.map((item) => {
											x.push(item.x_value.split('-')[2]);
											y.push(item.y_value);
										})

										that.secondShowButtonIndex = 'week';
										that.chartData = {
											categories: [...x],
											series: [{
												name: '使用时长',
												data: y,
												color: '#13C2C2',
											}]
										};
										return new Promise(function(resolve, reject) {
											uni.request({
												url: `http://${getApp().globalData.http}/app/data/today/work/time`,
												header: {
													'Authorization': getApp()
														.globalData.token,
												},
												success: (res) => {
													console.log(res);

													if (res.data.code == 0) {
														resolve(res.data
															.value);
													} else {
														uni.showToast({
															title: res
																.data
																.message,
															icon: 'none',
															duration: 2000
														});
													}


												}
											})
										})
									}).then(res => {
										console.log("工作时间")
										console.log(res);
										that.grade = res.health_number;
										that.workTimeHour = parseInt(res.work_time / 3600);
										that.workTimeMins = parseInt(res.work_time % 3600 / 60);
										that.continueWorkTimeHour = res.continuous_work_time.split(
											':')[0]-0;
										that.continueWorkTimeMins = res.continuous_work_time.split(
											':')[1]-0;
										let s, sH, sM, number;
										let e;
										let startChange, startChangeH, startChangeM;
										let startIndex = 0;
										let eeendTime;
										if (res.work_time_list.length) {
											s = res.work_time_list[0].start_time;
											sH = s.split(":")[0];
											sM = s.split(":")[1];
										}
										startChange = s;
										startChangeH = sH;
										startChangeM = sM;
										if (res.work_time_list.length > 0) {
											let lastEndIndex;
											that.boxArray.fill(0)
											res.work_time_list.map((item, index) => {
												console.log(startChangeH + "-" +
													startChangeM)
												// let start = item.start_time;
												// let startH = start.split(":")[0];
												// let startM = start.split(":")[1];
												// let end = item.end_time;
												// let endH = end.split(":")[0];
												// let endM = end.split(":")[1];
												// let H = startH - sH;
												// let M = startM - sM;
												// let int = 0;
												// let float = 0;
												// let q, w;
												// let startIndex, endIndex;
												// let indexOne, indexTwo;
												// let xx, xxx;

												// if (M < 0) {
												// 	M = M + 60;
												// 	H = H - 1;
												// }


												// int = (H * 60 + M) / 30;
												// float = (H * 60 + M) % 30;
												// startIndex = Math.ceil(int);


												// H = endH - sH;
												// M = endM - sM;
												// if (M < 0) {
												// 	M = M + 60;
												// 	H = H - 1;
												// }
												// int = (H * 60 + M) / 30;
												// float = (H * 60 + M) % 30;
												// endIndex = Math.ceil(int);
												// lastEndIndex = Math.ceil(int);

												// if (endIndex - startIndex > 3) {
												// 	number = 2;
												// } else {
												// 	number = 1;

												// }
												// if (endIndex > that.boxArray.length) {
												// 	that.boxArray.length = endIndex;

												// }
												// console.log(that.boxArray)
												// console.log('s' + startIndex);
												// console.log('e' + endIndex);
												// that.boxArray.fill(number, startIndex,
												// 	endIndex);

												let start = item.start_time;
												let startH = start.split(":")[0];
												let startM = start.split(":")[1];
												let end = item.end_time;
												let endH = end.split(":")[0];
												let endM = end.split(":")[1];

												let int, color;
												let endIndex;
												let H;
												let M;

												if (index == 0) {
													startIndex = 0;
												} else {
													H = startH - startChangeH;
													M = startM - startChangeM;

													if (M < 0) {
														M = M + 60;
														H = H - 1;
													}
													int = (H * 60 + M) / 30;
													endIndex = Math.ceil(int);



													if ((startIndex + endIndex) > that
														.boxArray.length) {
														that.boxArray.length = (
															startIndex + endIndex);

													}

													that.boxArray.fill(0, startIndex,
														(startIndex + endIndex));
													console.log(startIndex + "-----" + (
														startIndex + endIndex));
													startIndex = (startIndex + endIndex);
													startChangeH = startH;
													startChangeM = startM;

												}
												console.log(startChangeH + "" +
													startChangeM)

												H = endH - startChangeH;
												M = endM - startChangeM;

												if (M < 0) {
													M = M + 60;
													H = H - 1;
												}
												int = (H * 60 + M) / 30;
												endIndex = Math.ceil(int);
												console.log(startIndex + "----" + (
													startIndex + endIndex));
												if ((
														startIndex + endIndex) -
													startIndex > 3) {
													number = 2;
												} else {
													number = 1;

												}
												if ((startIndex + endIndex) > that.boxArray
													.length) {
													that.boxArray.length = (startIndex +
														endIndex);
												}
												lastEndIndex = (startIndex + endIndex)
												that.boxArray.fill(number, startIndex,
													(startIndex + endIndex));

												startIndex = (startIndex + endIndex);
												startChangeH = endH;
												startChangeM = endM;


												eeendTime = item.end_time;
											})
											console.log("------------" + lastEndIndex)
											that.boxArray.fill(4, lastEndIndex);



											let space;
											let spaceArray = [0, 1 / 3, 2 / 3, 3 / 3];


											space = (that.boxArray.length * 0.5).toFixed(1)
											console.log(space);
											spaceArray.map((item, index) => {
												if (index == 0) {
													that.timelist[index] = s;
												} else {
													let x = (space * item).toFixed(1);
													let h = (parseInt(x.split('.')[0]) +
														parseInt(sH));
													let m = parseInt(x.split('.')[1] * 6) +
														parseInt(sM)
													if (m >= 60) {
														h += 1;
														m -= 60;

													}
													if (h >= 24) {
														h -= 24;
													}
													that.timelist[index] = h + ":" + m;

													console.log(that.timelist[index])


												}



											})
											if (that.boxArray.length > 18) {
												that.timelist[3] = eeendTime;

											}

											console.log(that.timelist)
											that.timelist.map((item, index) => {

												if (that.timelist[index].length < 5) {
													let split = item.split(":");
													let x = split[0];
													let xx = split[1];
													console.log(split);
													if (split[0].length < 2) {
														x = "0" + split[0];
														console.log(x);
													}
													if (split[1].length < 2) {
														xx = "0" + split[1];
														console.log(xx);
													}
													that.timelist[index] = x + ':' + xx;
												}

											})
											console.log(that.timelist)







											// that.timelistTwo = +':' + sM;
											// that.timelistThree = +':' + sM;

										} else {
											that.boxArray.fill(4)
										}
										// let obj = uni.createSelectorQuery().select('.top-box');
										//    obj.boundingClientRect(function (data) { 
										// 				console.log(data.width)
										// 				that.width=data.width;
										// 						}).exec()
										console.log(that.boxArray.length)


										that.width = 750 * (that.boxArray.length * 16 + that
											.boxArray.length - 1) / 375;
										console.log(that.width)


									})

							}
						})
					}

				},
				fail() {
					uni.navigateTo({
						url: '../login/login'
					})
				}
			})





		},
		onShow() {

		},
		methods: {
			reserveClick() {
				if (this.list.length >= 2) {
					uni.showToast({
						title: '最多预约两个工位',
						icon: 'none',
						duration: 2000
					})
				} else {
					uni.navigateTo({
						url: '../reserve/reserve'
					})
				}

			},
			tarbarListen(index) {
				this.index = index;
				if (index == 1) {

					new Promise(function(resolve, reject) {

						uni.request({
							url: `http://${getApp().globalData.http}/app/data/fix/power/today/statistics`,

							header: {
								'Authorization': getApp().globalData.token,
							},
							success: (res) => {
								if (res.data.code == 0 && res.data.value.length != 0) {
									resolve(res.data.value);
								} else if (res.data.code != 0) {
									reject();
									uni.showToast({
										title: res.data.message,
										icon: 'none',
										duration: 2000
									});
								}
							}
						})
					}).then(res => {

						let x = [];
						let y = [];
						res.map((item, index) => {

							if (parseInt((index + 1) % 4) == 0 || index == 0) {
								x.push(item.x_value);
							} else {
								x.push(" ");
							}


							y.push(item.y_value);
						})
						console.log("???")
						console.log(x)
						this.reserveChartData = {
							categories: [...x],
							series: [{
								name: '使用量',
								data: y,
								color: '#FFF',
							}]
						};
					})

				} else if (index == 2) {
					new Promise(function(resolve, reject) {

						uni.request({
							url: `http://${getApp().globalData.http}/app/data/today/hour/power`,

							header: {
								'Authorization': getApp().globalData.token,
							},
							success: (res) => {
								if (res.data.code == 0 && res.data.value.length != 0) {
									resolve(res.data.value);
								} else if (res.data.code != 0) {
									reject();
									uni.showToast({
										title: res.data.message,
										icon: 'none',
										duration: 2000
									});
								}


							}
						})



					}).then(res => {
						let x = [];
						let y = [];
						res.map((item, index) => {
							if (((index + 1) == 1) || ((index + 1) % 5 == 0) || (index == res
									.length -
									1)) {
								x.push(item.x_value);
							} else {
								x.push(" ");
							}
							y.push(item.y_value);
						})

						this.SafeTodayChartData = {
							categories: [...x],
							series: [{
								name: '使用量',
								data: [...y],
								color: '#13C2C2',
							}]
						}
						return new Promise(function(resolve, reject) {

							uni.request({
								url: `http://${getApp().globalData.http}/app/data/power/statistics?type=${1}`,

								header: {
									'Authorization': getApp().globalData.token,
								},
								success: (res) => {

									if (res.data.code == 0) {
										resolve(res.data.value);
									} else if (res.data.code == -100) {
										uni.showToast({
											title: '请求失败',
											icon: 'none',
											duration: 2000
										});
									}


								}
							})



						})
					}).then(res => {
						let x = [];
						let y = [];
						res.map((item) => {
							x.push(item.x_value.split('-')[2]);
							y.push(item.y_value);
						})

						if (this.secondShowSafeButtonIndex == 'week') {
							this.SafeChartData = {
								categories: [...x],
								series: [{
									name: '使用量',
									data: [...y],
									color: '#13C2C2',
								}]
							}
						}

					})
				} else if (index == 3) {
					console.log(3);
					let that = this;
					uni.getLocation({
						type: 'gcj02',
						geocode: true,
						success: function(res) {
							console.log(res);
							that.place = res.address.city + ' ' + res.address.district;
							let id = res.address.district;
							uni.request({
								url: `http://${getApp().globalData.http}/app/data/city/${id}`,

								header: {
									'Authorization': getApp().globalData.token,
								},
								success: (res) => {
									console.log('1111111')
									console.log(res);

									that.weatherBox = res.data.value.data.forecast;
									console.log(res.data.value.data.forecast);

									that.environmentes[0].number = res.data.value.data
										.shidu.split("%")[0];
									res.data.value.data.shidu = res.data.value.data.shidu
										.split(
											"%")[0] / 100;
									console.log(res.data.value.data.shidu)
									if (res.data.value.data.shidu > 0.45 && res.data.value
										.data
										.shidu < 0.65) {
										that.environmentes[0].quality = '潮湿'
									} else {
										that.environmentes[0].quality = '干燥'
									}
									console.log(res.data.value.data.pm25)
									that.environmentes[2].number = res.data.value.data.pm25;
									if (res.data.value.data.pm25 < 75) {
										that.environmentes[2].quality = '优'
									} else {
										that.environmentes[2].quality = '差'
									}

									that.environmentes[1].number = res.data.value.data
										.wendu;
									if (res.data.value.data.wendu > 18 && res.data.value
										.data
										.wendu < 23) {
										that.environmentes[1].quality = '优'
									} else {
										that.environmentes[1].quality = '差'
									}


								},

							})




						},
						fail: function() {
							uni.showToast({
								title: '获取定位失败请确保打开了GPS定位',
								icon: 'none',
								duration: 2000,
							})
						}

					});

				}


			},
			doubleButtonListen(index) {
				let that = this;
				this.buttonIndex = index;

				if (this.buttonIndex == 0) {
					uni.request({
						url: `http://${getApp().globalData.http}/app/office/fixed/station/info`,
						// url: 'http://82.157.34.130:9901/app/office/fixed/station/info',
						header: {
							'Authorization': getApp().globalData.token,
						},
						success: (res) => {

							if (!res.data.value) {

								that.fixedTag = false;
							} else {

								that.fixedObj = res.data.value;
								that.powerFixedButton = !res.data.value.power_status;
								if (that.powerFixedButton == false) {
									that.fixedItem.chinese = '电源开启'
									that.fixedItem.english = 'On'
								} else {
									that.fixedItem.chinese = '电源关闭'
									that.fixedItem.english = 'Off'
								}
							}
						}
					})
				} else {
					uni.request({
						url: `http://${getApp().globalData.http}/app/office/reserve/station/list`,
						// url: 'http://82.157.34.130:9901/app/office/reserve/station/list',
						header: {

							'Authorization': getApp().globalData.token,
						},
						success: (res) => {

							if (Array.isArray(res.data.value) && res.data.value
								.length != 0) {
								that.list = res.data.value;
								that.infoTag = true;
								let arr = new Array([res.data.value.length]);
								arr.fill(true);
								that.listArr = arr;
								console.log(that.listArr)
								// that.resever = true;


							}

						}
					})
					uni.request({
						url: `http://${getApp().globalData.http}/app/office/reserve/station/info`,
						// url: 'http://82.157.34.130:9901/app/office/reserve/station/info',
						header: {
							'Authorization': getApp().globalData.token,
						},
						success: (res) => {

							if (res.data.value) {
								that.resever = true;
								that.sign = false;
								res.data.value.start_time=res.data.value.start_time.split(":")[0]+":"+res.data.value.start_time.split(":")[1]
								res.data.value.end_time=res.data.value.end_time.split(":")[0]+":"+res.data.value.end_time.split(":")[1]
								console.log("+++++++")
								console.log(res.data.value.start_time);
								that.infoObj = res.data.value;
								that.sign = res.data.value.sign_status;
								that.powerButton = !res.data.value
									.power_status;


							}

						}
					})
				}
			},
			powerButtonClick(id) {
				let that = this;
				this.powerButton = !this.powerButton;
				uni.request({
					url: `http://${getApp().globalData.http}/app/office/config/power/status?device_id=${id}&power_status=${!that.powerButton}`,
					// url: `http://82.157.34.130:9901/app/office/config/power/status?device_id=${id}&power_status=${!that.powerButton}`,

					header: {
						'Authorization': getApp().globalData.token,

					},
					success: (res) => {


					}
				})
				if (!this.powerButton) {
					this.item = {
						english: 'On',
						chinese: '电源开启',
						src: true
					}
				} else if (this.powerButton) {
					this.item = {
						english: 'Off',
						chinese: '电源关闭',
						src: false
					}
				}

			},
			powerFixedButtonClick(id) {
				let that = this;
				this.powerFixedButton = !this.powerFixedButton;
				uni.request({
					url: `http://${getApp().globalData.http}/app/office/config/power/status?device_id=${id}&power_status=${!that.powerFixedButton}`,
					// url: `http://82.157.34.130:9901/app/office/config/power/status?device_id=${id}&power_status=${!that.powerFixedButton}`,

					header: {
						'Authorization': getApp().globalData.token,

					},
					success: (res) => {
						if (res.data.code == 0) {

						} else if (res.data.code == -100) {
							uni.showToast({
								title: '请求失败',
								icon: 'none',
								duration: 2000
							});
						}

					}
				})
				if (!this.powerFixedButton) {
					this.fixedItem = {
						english: 'On',
						chinese: '电源开启',
						src: '/static/app/chazuo@2X.png'
					}
				} else if (this.powerFixedButton) {
					this.fixedItem = {
						english: 'Off',
						chinese: '电源关闭',
						src: '/static/app/chazuo@2X.png'
					}
				}

			},
			confirmListen() {
				this.warnShow = false;
			},
			newsClickListen() {

				uni.navigateTo({
					url: `../newsWarn/newsWarn?size=${this.count}`
				})
			},
			monthClick() {
				this.secondShowButtonIndex = 'month';
				uni.showLoading({
					title: '加载中'
				})
				new Promise(function(resolve, reject) {
					uni.request({
						url: `http://${getApp().globalData.http}/app/data/sit/statistics?type=${2}`,

						header: {
							'Authorization': getApp().globalData.token,
						},
						success: (res) => {

							if (res.data.code == 0) {
								resolve(res.data.value);
							} else {
								uni.showToast({
									title: '请求失败',
									icon: 'none',
									duration: 2000
								});
							}



						}
					})


				}).then(res => {
					let x = [];
					let y = [];


					res.map((item, index) => {
						if (((index + 1) == 1) || ((index + 1) % 5 == 0) || (index == res.length -
								1)) {
							x.push(item.x_value.split('-')[2]);
						} else {
							x.push(" ");
						}
						y.push(item.y_value);
					})


					this.chartData = {
						categories: [...x],
						series: [{
							name: '使用时长',
							data: y,
							color: '#13C2C2',
						}]
					};
					uni.hideLoading()
				})


			},
			weekClick() {
				this.secondShowButtonIndex = 'week';
				uni.showLoading({
					title: '加载中'
				})

				new Promise(function(resolve, reject) {
					uni.request({
						url: `http://${getApp().globalData.http}/app/data/sit/statistics?type=${1}`,

						header: {
							'Authorization': getApp().globalData.token,
						},
						success: (res) => {


							if (res.data.code == 0) {
								resolve(res.data.value);
							} else if (res.data.code == -100) {
								uni.showToast({
									title: '请求失败',
									icon: 'none',
									duration: 2000
								});
							}


						}
					})


				}).then(res => {
					let x = [];
					let y = [];

					res.map((item) => {
						x.push(item.x_value.split('-')[2]);
						y.push(item.y_value);
					})


					this.chartData = {
						categories: [...x],
						series: [{
							name: '使用时长',
							data: y,
							color: '#13C2C2',
						}]
					};
					uni.hideLoading()
				})


			},
			safeMonthClick() {
				this.secondShowSafeButtonIndex = 'month';
				uni.showLoading({
					title: '加载中'
				})

				new Promise(function(resolve, reject) {
					uni.request({
						url: `http://${getApp().globalData.http}/app/data/power/statistics?type=${2}`,
						header: {
							'Authorization': getApp().globalData.token,
						},
						success: (res) => {

							if (res.data.code == 0) {
								resolve(res.data.value);
							} else if (res.data.code == -100) {
								uni.showToast({
									title: '请求失败',
									icon: 'none',
									duration: 2000
								});
							}
						}
					})

				}).then(res => {
					let x = [];
					let y = [];
					res.map((item, index) => {
						if (((index + 1) == 1) || ((index + 1) % 5 == 0) || (index == res.length -
								1)) {
							x.push(item.x_value.split('-')[2]);
						} else {
							x.push(" ");
						}
						y.push(item.y_value);
					})

					this.SafeChartData = {
						categories: [...x],
						series: [{
							name: '使用量',
							data: [...y],
							color: '#13C2C2',
						}]
					}
					uni.hideLoading()
				})

			},
			safeWeekClick() {
				this.secondShowSafeButtonIndex = 'week';
				uni.showLoading({
					title: '加载中'
				})
				new Promise(function(resolve, reject) {
					uni.request({
						url: `http://${getApp().globalData.http}/app/data/power/statistics?type=${1}`,
						header: {
							'Authorization': getApp().globalData.token,
						},
						success: (res) => {

							if (res.data.code == 0) {
								resolve(res.data.value);
							} else if (res.data.code == -100) {
								uni.showToast({
									title: '请求失败',
									icon: 'none',
									duration: 2000
								});
							}
						}
					})

				}).then(res => {
					let x = [];
					let y = [];
					res.map((item) => {
						x.push(item.x_value.split('-')[2]);
						y.push(item.y_value);
					})

					this.SafeChartData = {
						categories: [...x],
						series: [{
							name: '使用量',
							data: [...y],
							color: '#13C2C2',
						}]
					}
					uni.hideLoading()
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
			stopInfo(id, index) {
				this.stopInfoId = id;
				this.stopInfoIndex = index;
				this.stopInfoShow = true;



			},
			stopInfoConfrim() {
				if (this.stopInfoId == -1 && this.stopInfoIndex == -1) {
                   uni.showToast({
                   	title:'取消失败',
					icon:'none'
                   })
				} else {
					let that = this;
					this.list.splice(this.stopInfoIndex, 1);
					that.stopInfoShow = false;
					uni.request({
						url: `http://${getApp().globalData.http}/app/office/delete/reserve/${that.stopInfoId}`,
						// url: `http://82.157.34.130:9901/app/office/delete/reserve/${id}`,
						method: 'DELETE',
						header: {
							'Authorization': getApp().globalData.token,
						},
						success: (res) => {

							if (res.data.code == 0) {
								
							} else if (res.data.code == -100) {
								uni.showToast({
									title: '请求失败',
									icon: 'none',
									duration: 2000
								});
							}

						}
				 });
					if (this.list.length == 0) {
						this.infoTag = false;
					}
				}

			},
			stopInfocancel() {
				this.stopInfoId = -1;
				this.stopInfoIndex = -1;
				this.stopInfoShow = false;
			},
			reserveStop(id) {
				this.reserveModal = true;
				this.reserveModalId = id;



			},
			reserveStopCancal() {
				this.reserveModal = false;
			},
			reserveStopConfirm() {
				let that = this;

				uni.request({
					url: `http://${getApp().globalData.http}/app/office/delete/reserve/${that.reserveModalId}`,
					// url: `http://82.157.34.130:9901/app/office/delete/reserve/${id}`,
					method: 'DELETE',
					header: {

						'Authorization': getApp().globalData.token,
					},
					success: (res) => {



						if (res.data.code == 0) {
							this.resever = !this.resever;
							that.infoObj = {};
						} else {
							uni.showToast({
								title: res.data.message,
								icon: 'none',
								duration: 2000
							});
						}


					}
				})
				this.reserveModal = false;
			},
			signIn(id) {
				uni.scanCode({
					scanType: ['qrCode'],
					success: function(res) {
						if (res.result.indexOf('device') != -1) {
							let start = res.result.indexOf('device') + 'device'.length;
							res.result = res.result.substring(start);

							console.log(res.result)
							uni.request({
								url: `http://${getApp().globalData.http}/app/office/sign/reserve?id=${id}&deviceNumber=${res.result}`,
								// url: `http://82.157.34.130:9901/app/office/sign/reserve?id=${id}&deviceNumber=${'ass1119'}`,
								header: {
									'Authorization': getApp().globalData.token,
								},
								success: (res) => {
									console.log(res.data.code);
									if (res.data.code == 0) {
										this.sign = true;


										uni.navigateTo({
											// url: `../login-success/login-success?resever=true&index=1&buttonIndex=1&startTime=${obj.startTime}&endTime=${obj.endTime}&position=${obj.position}`
											url: `../login-success/login-success?index=1&buttonIndex=1`
										});

									} else {
										uni.showToast({
											title: res.data.message,
											icon: 'none',
											duration: 2000
										});
									}

								}
							})
						} else {
							uni.showToast({
								title: '二维码无效',
								icon: 'none',
								duration: 2000
							});
						}

					}
				});

			},
			addUseTime() {
				this.addUse = true;

			},
			useStop(id) {
				this.useStopModalId = id;
				this.useStopModal = true;

			},
			useStopCancel() {
				this.useStopModal = false;
			},
			useStopConfirm() {
				let that = this;
				this.sign = !this.sign;
				this.resever = !this.resever;
				uni.request({
					url: `http://${getApp().globalData.http}/app/office/advance/end/${that.useStopModalId}`,
					// url: `http://82.157.34.130:9901/app/office/advance/end/${id}`,
					header: {
						'Authorization': getApp().globalData.token,
					},
					success: (res) => {
						if (res.data.code === 0) {
							uni.request({
								url: `http://${getApp().globalData.http}/app/message/not/read/count`,
								// url: 'http://82.157.34.130:9901/app/message/not/read/count',
								header: {
									'Authorization': getApp().globalData.token,
								},
								success: (res) => {

									if (res.data.code === 0) {

										that.showMessage.showMessage = res.data
											.value;
										that.showMessage.show = true;

										that.count = res.data.value;
										that.headerShow = true;
										that.infoObj = {};
									} else {
										uni.showToast({
											title: res.data.message,
											icon: 'none',
											duration: 2000
										});
									}
								}
							})
						} else {
							uni.showToast({
								title: res.data.message,
								icon: 'none',
								duration: 2000
							});
						}


					}
				})

				this.useStopModal = false;
			},
			addCancal() {
				this.addUse = false;

			},
			addUseTimeChose(e) {
				this.e = e.detail.value;
			},

			addSubmit() {
				this.addIndex = this.e;
				this.addUse = false;
				let minute = this.timeArray[this.addIndex];
				let id = this.infoObj.id;
				let that = this;
				uni.request({
					url: `http://${getApp().globalData.http}/app/office/extend/use?id=${id}&minute=${minute}`,
					// url: `http://82.157.34.130:9901/app/office/extend/use?id=${id}&minute=${minute}`,
					header: {
						'Authorization': getApp().globalData.token,
					},
					success: (res) => {
						if (res.data.code == 0) {
							uni.request({
								url: `http://${getApp().globalData.http}/app/office/reserve/station/info`,
								// url: 'http://82.157.34.130:9901/app/office/reserve/station/info',
								header: {
									'Authorization': getApp().globalData.token,
								},
								success: (res) => {
									console.log("延长使用")
									console.log(res);
									if (res.data.code == 0) {
										if (res.data.value != null) {
											res.data.value.start_time=res.data.value.start_time.split(":")[0]+":"+res.data.value.start_time.split(":")[1]
											res.data.value.end_time=res.data.value.end_time.split(":")[0]+":"+res.data.value.end_time.split(":")[1]
											console.log("+++++++")
											console.log(res.data.value.start_time);
											that.infoObj = res.data.value;
											that.sign = res.data.value.sign_status;
											that.powerButton = !res.data.value.power_status;

										} else {
											uni.showToast({
												title: '不支持延长时间',
												icon: 'none',
												duration: 2000
											});
										}

									} else {
										uni.showToast({
											title: res.data.message,
											icon: 'none',
											duration: 2000
										});
									}

								}
							})
						} else {
							uni.showToast({
								title: res.data.message,
								icon: 'none',
								duration: 2000
							});
						}


					}
				})

			},
			scanClickListen() {
				console.log(this.infoObj)
				if (this.list.length >= 2 || (JSON.stringify(this.infoObj) != "{}" && this.list.length == 1)) {
					uni.showToast({
						title: '最多预约两个工位',
						icon: 'none',
						duration: 2000
					});
				} else {

					uni.scanCode({
						scanType: ['qrCode'],
						success: function(res) {
							console.log('条码类型：' + res.scanType);
							console.log('条码内容：' + res.result);
							console.log(res.result);
							if (res.result.indexOf('device') != -1) {
								let start = res.result.indexOf('device') + 'device'.length;
								res.result = res.result.substring(start);


								uni.request({
									url: `http://${getApp().globalData.http}/app/office/station/info/${res.result}`,
									// url: `http://82.157.34.130:9901/app/office/station/info/${res.result}`,
									header: {
										'Authorization': getApp().globalData.token,
									},
									success: (res) => {
										if (res.data.code == 0) {
											uni.navigateTo({
												url: `../../pages/reserve/codeReserve?id=${res.data.value.id}&station_number=${res.data.value.station_number}`
											})
										} else {
											uni.showToast({
												title: res.data.message,
												icon: 'none',
												duration: 2000,
											})
										}



									}
								})
							} else {
								uni.showToast({
									title: '二维码无效',
									icon: 'none',
									duration: 2000
								});
							}

						}
					});
				}
			},
			getWeatherSrc(type) {

				if (type == '晴') {
					return this.weatherImgArray[1];
				} else if (type == '多云') {
					return this.weatherImgArray[2];
				} else if (type == '阴') {
					return this.weatherImgArray[3];
				} else if (type == '阵雨') {
					return this.weatherImgArray[4];
				} else if (type == '暴雨' || type == '大到暴雨') {
					return this.weatherImgArray[5];
				} else if (type == '大暴雨' || type == '暴雨到大暴雨') {
					return this.weatherImgArray[6];
				} else if (type == '雷阵雨及伴有冰雹') {
					return this.weatherImgArray[7];
				} else if (type == '特大暴雨' || type == '大暴雨到特大暴雨') {
					return this.weatherImgArray[8];
				} else if (type == '小雨' || type == '小到中雨') {
					return this.weatherImgArray[9];
				} else if (type == '大雨') {
					return this.weatherImgArray[10];
				} else if (type == '雷阵雨') {
					return this.weatherImgArray[11];
				} else if (type == '雨夹雪') {
					return this.weatherImgArray[12];
				} else if (type == '中雨' || type == '中到大雨') {
					return this.weatherImgArray[13];
				} else if (type == '大雪') {
					return this.weatherImgArray[14];
				} else if (type == '小雪' || type == '小到中雪') {
					return this.weatherImgArray[15];
				} else if (type == '中雪' || type == '中到大雪') {
					return this.weatherImgArray[16];
				} else if (type == '阵雪') {
					return this.weatherImgArray[17];
				} else if (type == '暴雪' || type == '大到暴雪') {
					return this.weatherImgArray[18];
				} else if (type == '强沙尘暴') {
					return this.weatherImgArray[19];
				} else if (type == '浮尘') {
					return this.weatherImgArray[20];
				} else if (type == '雾') {
					return this.weatherImgArray[21];
				} else if (type == '沙尘暴') {
					return this.weatherImgArray[22];
				} else if (type == '冻雨') {
					return this.weatherImgArray[23];
				} else if (type == '扬沙') {
					return this.weatherImgArray[24];
				} else if (type == '霾') {
					return this.weatherImgArray[25];
				}


				return this.imgArray[0]

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
					return '../../static/app/sad.svg'
				} else if (this.grade > 33 && this.grade <= 66) {
					return '	../../static/app/dull.svg'
				} else if (this.grade > 66)
					return '	../../static/app/happy.svg'
			},

			getButtonIndex() {

				return this.buttonIndex
			},


		},

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

	#messageTwo {

		background-image: url(../../static/app/messageTwo.svg);
		background-size: cover;
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
		background-image: url(../../static/app/logo.svg);
		background-repeat: no-repeat;
		background-position: center calc(100vh * 18/812);
		background-size: calc(750rpx * 48/ 375) calc(100vh * 48/ 812);


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

	.content-main .firstShow .firstShow-center {

		/* 	height: calc(100vh * 74/812); */


	}

	.content-main .firstShow .firstShow-center #top-box {

		width: calc(750rpx * 305/ 375);
		display: flex;
		/* margin: calc(100vh * 24/812)  calc(750rpx * 19/ 375); */
		margin-top: calc(100vh * 24/812);
		margin-bottom: calc(100vh * 8/812);
		margin-left: calc(750rpx * 19/ 375);
		margin-right: calc(750rpx * 19/ 375);



	}

	.content-main .firstShow .firstShow-center #top-box .item {

		flex-grow: 0;
		flex-shrink: 0;
		width: calc(750rpx * 16/ 375);
		height: calc(100vh * 16/812);
		margin-right: calc(750rpx * 1 / 375);

	}

	.content-main .firstShow .firstShow-center #top-box .colorOne {
		background-color: #D0F3F3;
	}

	.content-main .firstShow .firstShow-center #top-box .colorTwo {
		background-color: #71DADA;
	}

	.content-main .firstShow .firstShow-center #top-box .colorThree {
		background-color: #FB696C;
	}

	.content-main .firstShow .firstShow-center #top-box .colorFour {
		background-color: #EDEFF2;
	}

	.content-main .firstShow .firstShow-center #top-box .item:last-child {
		margin-right: 0;
	}



	.content-main .firstShow .firstShow-center .center-box {

		width: 100%;
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
		width: calc(750rpx * 6/ 375);
		height: calc(750rpx * 6/ 375);
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
		width: calc(750rpx * 6/ 375);
		height: calc(750rpx * 6/ 375);
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
		width: calc(750rpx * 6/ 375);
		height: calc(750rpx * 6/ 375);
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
		background: linear-gradient(270deg, #FBD66A 0%, #FEB97A 20%, rgba(251, 105, 108, 0.80) 100%);

	}

	.content-main .firstShow .firstShow-bottom-bottom .top .long:nth-child(3) {
		margin-left: calc(750rpx * 1/ 375);
		background: linear-gradient(90deg, #FFE862 0%, #B4EE6B 100%);
		;
	}

	.content-main .firstShow .firstShow-bottom-bottom .top .long:nth-child(4) {
		border-top-right-radius: calc(750rpx * 100/ 375);
		border-bottom-right-radius: calc(750rpx * 100/ 375);
		margin-left: calc(750rpx * 1/ 375);
		background: linear-gradient(90deg, #A8EF69 3%, rgba(17, 174, 174, 0.80) 100%);
	}

	.content-main .secondShow {
		overflow: hidden;
		width: calc(750rpx * 343/ 375);
		height: calc(100vh * 259/812);
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
		width: calc(750rpx * 13.06/ 375);
		height: calc(100vh* 13.24/ 812);
		background-image: url(../../static/app/icon-zhishu@2X.png);
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

	.content-main .secondOffice .box {

		height: calc(100vh * 91/812);
		;
		overflow-y: scroll;

	}

	.content-main .secondOffice .box .secondOffice-item {
		margin-top: calc(100vh * 12/812);
		height: calc(100vh * 17/812);
		display: flex;
		justify-content: space-between;
		color: rgba(10, 32, 57, 0.7);
		font-size: calc(750rpx * 12/ 375);
	}

	.content-main .secondOffice .box .secondOffice-item:first-child {
		margin-top: calc(100vh * 16/812);
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
		top: 8%;
		left: 5%;
		width: calc(750rpx * 20/ 375);
		height: calc(100vh * 20/812);
		background-color: #F8F8F8;
		border-radius: 50%;
	}

	.content-main .reserveFirst .reserveFirst-right .reserveFirst-right-radius-click {
		position: absolute;
		top: 8%;
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

		margin: 0 calc(750rpx * 26/ 375) calc(750rpx * 22/ 375);
		padding-top: calc(100vh * 15/812);

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

	.bgcOne {
		background-image: url(../../static/app/chazuo@2X.png);
	}
</style>
