<template>
	<view>

		<map style="width:100%;height:85vh;" :latitude="latitude" :longitude="longitude" :markers="marker" 
			></map>

	</view>

</template>

<script>
	export default {

		data() {


			return {
				agentId: 0,
				title: 'map',
				latitude: 40.013305,
				longitude: 118.68571,
				marker: [{
					id: 0,
					latitude: 40.013305, //纬度
					longitude: 118.685713, //经度
					iconPath: '../../static/app/map.jpg', //显示的图标        
					rotate: 0, // 旋转度数
					width:111 , //宽
					height: 111, //高
					title: '你在哪了', //标注点名
					alpha: 0.5, //透明度
					label: {
						content: '唐山迁安', //文本
						color: 'red',

					}
				}],
			



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

</style>
