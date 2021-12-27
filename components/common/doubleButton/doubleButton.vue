<template>
	<view class="doubleButton">
		<view class="button" :class="{'click':number==index} " @click="buttonListen(index)"
			v-for="(item,index) in name">
			{{item}}
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
			name: {
				type: Array,
				default: function() {
					return ['固定', '共享']
				}

			},
			number: {
				type:Number,
			
			}
		},
		onLoad() {

		},
		methods: {
			buttonListen(index) {
				console.log('index'+'='+this.number);
				this.$emit('doubleButtonClick', index)
			}
		}
	}
</script>

<style>
	.doubleButton {
		width: calc(750rpx * 216 / 375);
		height: calc(100vh * 32/812);
		line-height: calc(100vh * 32/812);
		border-radius: calc(750rpx * 16 / 375);
		background-color: rgba(10, 32, 57, 0.1);
		margin-top: calc(100vh * 12/812);
		margin-left: calc(750rpx * 80 / 375);
		overflow: hidden;
		color: rgba(19, 194, 194, 1);
		font-size: calc(750rpx * 16 / 375);
	}

	.doubleButton .button:nth-child(1) {
		display: inline-block;
		width: calc(750rpx * 108/ 375);
		height: calc(100vh * 32/812);
		line-height: calc(100vh * 32/812);
		border-radius: calc(750rpx * 16 / 375);

		text-align: center;
	}

	.doubleButton .button:nth-child(1)::before {
		content: '';
		display: inline-block;
		width: calc(750rpx * 14/ 375);
		height: calc(750rpx * 14/ 375);

		background-image: url(../../../static/app/fixed.svg);
		background-size: contain;
		margin-right: calc(750rpx * 12/ 375);

		transform: translateY(15%);


	}

	.doubleButton .button:nth-child(2) {
		display: inline-block;
		width: calc(750rpx * 108/ 375);
		height: calc(100vh * 32/812);
		line-height: calc(100vh * 32/812);
		border-radius: calc(750rpx * 16 / 375);
		text-align: center;



	}

	.doubleButton .button:nth-child(2)::before {
		content: '';
		display: inline-block;
		width: calc(750rpx * 14/ 375);
		height: calc(750rpx * 14/ 375);
		background-image: url(../../../static/app/handReverse.svg);
		background-size: contain;
		margin-right: calc(750rpx * 12/ 375);

		transform: translateY(15%);


	}

	.click {
		background-color: rgba(255, 255, 255, 1);
	}
</style>
