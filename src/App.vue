<script>
	export default {
		onLaunch: function() {
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		},
		globalData:{
			uploadImgList:[],
			identifyImg:{imgSrc:'',imgInfo:''}
		}
	}
</script>

<style>

	/*每个页面公共css */
@import 'common/colorui/app.css'
</style>
