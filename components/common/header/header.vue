<template>
	<view class="header">
		<view class="left" @click="personalClick">
			<view v-show="showObj.show"
				style="	color: white;position: absolute;width:calc(750rpx * 20/ 375);height:calc(750rpx * 20	/ 375); text-align: center;font-size:calc(750rpx * 10/ 375) ;line-height:calc(750rpx * 20	/ 375);">
				<view
					style="width: 100%;height: 100%;margin-top:calc(100vh * -5/812);margin-left:calc(750rpx *  1/ 375);font-size: 16rpx;">

					<image src="../../../static/app/message.svg" style="width: 100%;height: 100%;"></image>
					<view
						style="width: 100%;height: 100%;position: absolute;top:-25%;font-size: 16rpx;text-align: center;	">
						{{showObj.showMessage}}
					</view>
				</view>


			</view>
			<slot name='left'>
				<image src="../../../static/app/mine_nol.svg"
					style="width:calc(750rpx * 26.42/ 375);height:calc(750rpx * 28.47	/ 375);"></image>
				<view class="personal" :class="{'show':!show}">
					<text @click="revisePasswordClick">修改密码</text>
					<text @click="newsClick" style="position: relative;">消息提醒<view v-show="showObj.show"
							style="width:calc(750rpx * 5/ 375);height:calc(750rpx * 5/ 375);position: absolute; background-color: #FB696C;right:10%;top: 20%;border-radius: 50%;">
						</view></text>

					<text @click="leaveClick">退出系统</text>
				</view>
			</slot>
		</view>
		<view class="center">
			<slot name='center'>

			</slot>
		</view>
		<view>
			<slot name='right'>
				<image src="../../../static/app/hand.svg"
					style="width:calc(750rpx * 21/ 375);height:calc(750rpx * 25.43/ 375);" @click="repairClick"></image>
				<image src="../../../static/app/scanning.svg"
					style="width:calc(750rpx * 21/ 375);height:calc(750rpx * 25.43/ 375);" @click="scanClick"></image>

			</slot>
		</view>




	</view>
</template>

<script>
	export default {
		data() {
			return {
				show: false,
			}
		},
		props: {
			showObj: {
				type: Object,
				default: function() {
					return {
						showMessage: 0,
						show: false,
					}
				}
			},


		},
		onLoad() {

		},
		methods: {
			personalClick() {
				this.show = !this.show;
			},
			revisePasswordClick() {
				uni.navigateTo({
					url: '../revisePassword/revisePassword'
				})
			},
			leaveClick() {
				uni.removeStorage({
					key: 'login',
					success: function(res) {
						console.log('success');
					}
				});

				uni.navigateTo({
					url: '../login/login'
				})

			},
			repairClick() {
				uni.navigateTo({
					url: '../repair/repair'
				})
			},
			scanClick() {

				this.$emit('scanClick')



			},
			newsClick() {
				if (this.showObj.show) {
					this.showObj.show = !this.showObj.show
				}
				this.$emit('newsClick')
			}
		},



	}
</script>

<style>
	.header {
		display: flex;

		justify-content: space-between;

	}

	.header view:nth-child(1) {

		margin-left: calc(750rpx * 16/ 375);

	}

	.header .center {
		height: 100%;

	}

	.header view:nth-child(3) image:nth-child(1) {

		margin-right: calc(750rpx * 16/ 375);

	}

	.header view:nth-child(3) image:nth-child(2) {
		margin-right: calc(750rpx * 14.67/ 375);

	}



	.personal {
		box-sizing: border-box;
		width: calc(750rpx *128/ 375);
		height: calc(100vh *138/812);
		margin-left: calc(-750rpx * 10/ 375);
		border-radius: calc(750rpx * 5/ 375);
		position: absolute;
		top: 10%;
		z-index: 1;
		background-color: rgba(255, 255, 255, 1);
		font-size: calc(750rpx * 15/ 375);
		display: flex;
		text-align: center;
		flex-direction: column;
		padding: calc(100vh *14.98/812) calc(750rpx *16/ 375) calc(100vh *13/812);

	}

	.personal text {
		width: calc(750rpx *96/ 375);
		height: calc(100vh *35/812);
		line-height: calc(100vh *35/812);
		color: rgba(17, 30, 54, 1);
	}

	.personal text:nth-child(2),
	text:nth-child(3) {
		border-top: calc(750rpx *0.5/ 375) solid rgba(237, 239, 242, 1);

	}

	.personal text:nth-child(2) {}

	.personal::before {
		content: '';
		position: absolute;
		top: -11%;
		left: 11%;
		width: 0;
		height: 0;
		border: calc(100vh *8/812) solid transparent;
		border-bottom-color: white;

	}

	.show {
		display: none;
	}
</style>
