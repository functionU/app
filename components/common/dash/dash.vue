<template>
	<view class="dash">

	</view>
</template>

<script>
	export default {
		data() {
			return {

			}
		},
		props: {


		},
		onLoad() {

		},
		methods: {

		}
	}
</script>

<style>
	.dash {
		margin-left: calc(750rpx * 24/ 375);
		margin-right: calc(750rpx * 24/ 375);
		
		height: calc(100vh * 1/ 812);
		background-image: linear-gradient(to right,
				#E6E7E8 0% 50%,
				transparent 50% 100%);
		background-size: calc(750rpx * 8/ 375) calc(100vh * 1/ 812);
		background-repeat:repeat-x;


	}
</style>
