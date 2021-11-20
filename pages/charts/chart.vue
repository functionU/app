<!-- <template>
	<view>
		
		<text>{{getText}}</text>
		<button @click="addClick">add 1</button>
		<button type="default" @click="get">jump</button>
	</view>

</template>

<script>
	export default {

		data() {


			return {
				text:"1",
			}
		},
		onLoad() {

		},
		methods: {
			addClick() {
				// getApp().globalData.text = getApp().globalData.text + 1;
				// console.log(getApp().globalData.text)
				getApp().globalData.text=	getApp().globalData.text+"abc";
				this.text=getApp().globalData.text;
			},
			get(){
				uni.navigateTo({
					url:"./charts"
				})
			}
		},
		components: {


		},
		computed: {
     getText(){
		 // return getApp().globalData.text;
		 return this.text;
	 }
		}
	}
</script>

<style>

</style>
 -->