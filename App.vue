
<script>
	export default {
		onLaunch: function() {
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')
			if (!getApp().globalData.token) {
				uni.navigateTo({
					url: "../login/login"
				})
			}
		},
		onHide: function() {
			console.log('App Hide')
		},
		globalData:{
			token:"",
			positionArray:[],
			usuallyArray:[],
			// http:'192.168.1.239:9900',
			http:'82.157.34.130:9901',
			
		}
	}
</script>


<style>
	/*每个页面公共css */
</style>
