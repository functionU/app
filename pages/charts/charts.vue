<!-- <template>
	<view >
	 {{text}}
	 <button type="default" @click="get">get</button>
	</view>


</template>
<script>
	

	export default {

		data() {
		

			return {
			text:getApp().globalData.text,
			}
		},
		onLoad() {

		},
		methods: {
get(){
	this.text=getApp().globalData.text
	console.log(getApp().globalData.text)
}
			
		},
		components: {
		

		},
		computed: {

		}
	}
</script>

<style>

</style>
 -->
<template>
	<view>
		<view class="page-body">
			<view class="map">
				<image src="../../static/app/map.jpg" mode=""></image>
				<view class="">

				</view>
			</view>
		</view>
	</view>
</template>
<script>
	export default {
		data() {
			return {

			}
		},

		methods: {

		},
		onload() {
	
			uni.request({
				url: `http://${getApp().globalData.http}/app/office/station/map/{floorId}`,
				header: {
					'Content-Type': 'application/json',
					'Authorization': getApp().globalData.token,
				},
			})
			uni.request({
				url: `http://${getApp().globalData.http}/app/office/map/empty/station/list`,
				header: {
					'Content-Type': 'application/json',
					'Authorization': getApp().globalData.token,
				},
			})
		}
	}
</script>
<style>
	.page-body {

		overflow: scroll;
	}

	.map {
		position: relative;
		height: 500px;
		width: 600px;
	}

	image {

		height: 1000px;
		width: 2000px;
	}
</style>
