<template>
	<view class="environment-item" >
		<text class="text">{{item.name}}</text>
		<image :src=item.src style="width: calc(750rpx * 22.33/ 375) ; height: calc(100vh * 24.23/812);"></image>
		<text>{{item.number}} {{item.name=='温度' ? '°C' : '' }}{{item.name=='PM2.5' ? 'μg/mg' : '' }}{{item.name=='二氧化碳' ? 'μg/m3' : '' }}{{item.name=='甲醛' ? 'μg/m3' : '' }}{{item.name=='噪音' ? 'db' : '' }}{{item.name=='湿度' ? '%' : '' }}</text>
		<view :class="{item,'good':item.quality=='优'||item.quality=='潮湿','bad':item.quality=='差'||item.quality=='干燥','none':item.quality=='无'}">
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
		color: black;
		text-align: center;
		background-color: rgba(255, 169, 54, 1);
		
	}

	.good {
		color: white;
		background-color: #1ABFC2;
	}

	.bad {
		color: white;
		background-color: #FB696C;
		
	}
	.none{
		background-color: white;
		
	}
</style>
