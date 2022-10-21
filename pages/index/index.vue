<template>
	<view>
		<image src="../../static/images/black_jack.png" style="width: 100%;margin-top: 20%;" mode="widthFix"></image>
		<button style="width: 50%;margin-top: 15%;" class="d-flex a-center j-center py-0 button" @tap="startGame">开始游戏</button>
	</view>

</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		onLoad() {
			uni.setStorageSync('betMoney',1000)
		},
		methods: {
			startGame(){
				console.log("开始游戏了");
				//调用开始游戏接口
				uni.navigateTo({
					url: '/pages/prepare/prepare',
					animationType:"zoom-out"
				});
			},
			continueGame(){
				console.log("继续游戏");
				uni.navigateTo({
					url: '/pages/prepare/prepare',
					animationType:"zoom-out"
				});
			}
		}
	}
</script>

<style>
	page{
		background-color: #719971;
		background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 80 80' width='80' height='80'%3E%3Cpath fill='%23466642' fill-opacity='0.4' d='M14 16H9v-2h5V9.87a4 4 0 1 1 2 0V14h5v2h-5v15.95A10 10 0 0 0 23.66 27l-3.46-2 8.2-2.2-2.9 5a12 12 0 0 1-21 0l-2.89-5 8.2 2.2-3.47 2A10 10 0 0 0 14 31.95V16zm40 40h-5v-2h5v-4.13a4 4 0 1 1 2 0V54h5v2h-5v15.95A10 10 0 0 0 63.66 67l-3.47-2 8.2-2.2-2.88 5a12 12 0 0 1-21.02 0l-2.88-5 8.2 2.2-3.47 2A10 10 0 0 0 54 71.95V56zm-39 6a2 2 0 1 1 0-4 2 2 0 0 1 0 4zm40-40a2 2 0 1 1 0-4 2 2 0 0 1 0 4zM15 8a2 2 0 1 0 0-4 2 2 0 0 0 0 4zm40 40a2 2 0 1 0 0-4 2 2 0 0 0 0 4z'%3E%3C/path%3E%3C/svg%3E");
	}
	.button {
	  padding: 20rpx 50rpx;
	  font-size: 35rpx;
	  cursor: pointer;
	  text-align: center;   
	  text-decoration: none;
	  outline: none;
	  color: #fff;
	  background-color: #3e8e41;
	  border: none;
	  border-radius: 15px;
	  box-shadow: 0 9px #999;
	}
	
	.button:hover {background-color: #3e8e41}
	
	.button:active {
	  background-color: #3e8e41;
	  box-shadow: 0 5px #666;
	  transform: translateY(4px);
	}

</style>
