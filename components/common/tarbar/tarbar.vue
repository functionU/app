<template>
	<view class="tarbar">
		<view v-for="(item,index) in tarbarNames" class="tarbar-item" :class="{'click':index==clickIndex}"
			@click="tarbarItemClick(index)">{{item}}</view>
	</view>
</template>
<script>
	export default {
		data() {
			return {
				title: 'Hello',


			}
		},
		props: {
			tarbarNames: {
				type: Array,
				default: function() {
					return ['健康办公', '工位预约', '安全用电', '办公环境']
				}
			},
			clickIndex: {
				type: Number,

			}
		},
		onLoad() {
			console.log("1")
		},
		methods: {
			tarbarItemClick(index) {
			
				this.$emit('tarbarClick', index)
			}
		}
	}
</script>

<style>
	.tarbar {
		display: flex;

		margin-top: calc(100vh * 42/812);
	}

	.tarbar-item {
		color: rgba(255, 255, 255, 0.6);
		font-size: calc(750rpx * 14/ 375);
		width: calc(750rpx * 94/ 375);
		height: calc(100vh * 44/812);
		line-height: calc(100vh * 44/812);
		text-align: center;
	}

	.click::before {
		content: '';
		display: block;
		width: calc(750rpx * 40/ 375);
		height: calc(100vh * 3/812);
		position: relative;
		left: 30%;
		background-color: #FFFFFF;

	}

	.click {
		color: white;


	}
</style>
