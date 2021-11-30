<template>
	<view>
		<view class="map" :class="{'transition':show}">

		</view>
		<button type="default" @click="btn">click</button>
	</view>

</template>

<script>
	export default {

		data() {


			return {
				show: false
			}
		},
		onLoad() {

		},
		methods: {
			btn() {
				this.show = !this.show;
			}


		},
		components: {


		}, 
		computed: {

		}
	}
</script>

<style>
	.map {
		position: absolute;
		left: 0;
		bottom: 0;
		width: 500px;
		height: 800px;
		background-color: #007AFF;
			transition: all 1s;
	}

	.transition {
	
		height: 0;
	}
</style>
