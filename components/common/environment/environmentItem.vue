<template>
	<view class="environment-item" >
		<text class="text">{{item.name}}</text>
		<image :src=item.src style="width: calc(750rpx * 22.33/ 375) ; height: calc(100vh * 24.23/812);"></image>
		<text>{{item.number}}</text>
		<view :class="{item,'good':item.quality=='优','bad':item.quality=='差'}">
			{{item.quality}}
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
				default: function() {
					return {
						name: '温度',
						src: '../../static/app/icon-zhengque.png',
						number: '11.11',
						quality: '差'
					}
				},

			}
		},
		onLoad() {

		},
		methods: {}
	}
</script>

<style>
	.environment-item {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		justify-content: space-around;
		width: calc(750rpx * 90/ 375);
		height: calc(100vh * 144/ 812);
		font-size: calc(750rpx * 12/ 375);
		border-radius: calc(750rpx * 10/ 375);
		border: 1px solid rgba(10, 32, 57, 0.15);




	}

	.text {
		font-weight: bold;

	}

	.item {
		width: calc(750rpx * 58/ 375);
		height: calc(100vh * 22/ 812);
		line-height: calc(100vh * 22/ 812);
		border-radius: calc(750rpx * 14/ 375);
		color: rgba(255, 255, 255, 1);
		text-align: center;
		background-color: rgba(255, 169, 54, 1);
	}

	.good {
		background-color: rgba(26, 191, 194, 1);
	}

	.bad {
		background-color: rgba(251, 105, 108, 1);
	}
</style>
