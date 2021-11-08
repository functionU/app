<template>
	<view class="tip">
         <view class="left">
         	<view class="tip-item-first">
				<image src="../../../static/app/icon-title@2X.png" style="width: calc(750rpx * 14/ 375);height: calc(750rpx * 14/ 375);"></image>
         		{{item.name}}
         	</view>
         </view>

		
				<view class="tip-item-second">
					<slot name="right">
					<image src="../../../static/app/info@2X.png" style="width: calc(750rpx * 11/ 375);height: calc(750rpx * 11/ 375);"></image>
					<slot name="second"> {{item.place}}</slot>
					</slot>
				</view>
		
	
	</view>
</template>

<script>
	export default {
		data() {
			return {

			}
		},
		props: {
			item: {
				type: Object,
				default: () => {
					return {
						name: '环境指数',
						place: '上海市 静安区'
					}
				}
			}

		},
		onLoad() {

		},
		methods: {

		}
	}
</script>

<style>
	.tip {

		display: flex;
		
		width: calc(750rpx * 343/ 375);
		height: calc(100vh * 24/812);
		margin-top: calc(100vh * 16 / 812);
	}

	.tip .tip-item-first {
		width: auto;
	   margin-left: calc(750rpx * 26/ 375);
		line-height:calc(100vh * 24/812);
		font-weight: bold;
		font-size: calc(750rpx * 14/ 375);
	}
 .tip image{
	margin-right:calc(750rpx * 8/ 375) ;
	transform: translateY(10%);
}
	.tip .tip-item-second {
		margin-left: auto;
        margin-right: calc(750rpx * 8/ 375);
		line-height:calc(100vh * 28/812);
		margin-right: calc(750rpx * 28/ 375);
		font-size: calc(750rpx *12/ 375);
		color: rgba(10, 32, 57, 0.5);
     
	}


</style>
