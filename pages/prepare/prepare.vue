<template>
	<view class="">
		<view class="" style="margin-top: 5%;">
			
			<view class="" style="height: 100rpx;display: flex;flex-direction: row;justify-content: center;font-size: 70rpx;margin-bottom: 40rpx;
			font-family: cursive;color: maroon;">total money:{{totalMoney}}</view>
			
		</view>
		<view class="" style="display: flex;">
			<view class="" style="margin-left: 30rpx;">
				<view class="">
					<image src="../../static/images/chip_one.png" mode="" style="width: 150rpx;height: 150rpx;" @click="tapOne"></image>
					<image src="../../static/images/chip_five.png" mode="" style="width: 150rpx;height: 150rpx;" @click="tapTwo"></image>
					<image src="../../static/images/chip_twenty_five.png" mode="" style="width: 150rpx;height: 150rpx;" @click="tapThree"></image>
				</view>
				<view class="">
					<image src="../../static/images/chip_fifty.png" mode="" style="width: 150rpx;height: 150rpx;" @click="tapFour"></image>
					<image src="../../static/images/chip_hundred.png" mode="" style="width: 150rpx;height: 150rpx;" @click="tapFive"></image>
				</view>
			</view>
			<view style="width: 180rpx;margin-left: 80rpx;;margin-top: 80rpx;height: 180rpx;font-size: 50rpx;font-family: cursive;background-color: black;color: #fff;border-radius: 100%;">
				<view style="margin-left: 30rpx;margin-top: 65rpx;font-size: 40rpx;">
					Bet:{{betMoney}}
				</view>
			</view>
		</view>
		<button class="button" style="width: 40%;margin-top: 10rpx;" @click="deal">Restart</button>
		<button class="button" style="width: 40%;margin-top: 50rpx;" @click="continueGame">Continue</button>
		<button class="button" style="margin-top: 50rpx;width: 40%;" @click="clear">Clear</button>
		<button class="button" style="margin-top: 50rpx;width: 40%;" @click="allIn">All in</button>
		<button class="button" style="margin-top: 50rpx;width: 40%;" @click="returnHome">Home</button>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				one:0,
				two:0,
				three:0,
				four:0,
				five:0,
				totalMoney:uni.getStorageSync('betMoney'),
				betMoney:0,
				res:{
					
				}
			}
		},
		methods: {
			continueGame(){
				if(this.betMoney===0){
					uni.showToast({
						title: '您应该赌一些钱~',
						icon:"none"
					});
				}
				else{
					console.log("开始游戏");
					uni.request({
						url: 'http://192.168.1.104:8080/continueGame',
						method: 'GET',
						sslVerify:false,
						data: {
							ante:this.betMoney
						},
						success: res => {
							this.res=res
							console.log(this.res);
							console.log(this.betMoney);
							uni.navigateTo({
								url: '/pages/game/game?bet='+this.betMoney+'&res='+encodeURIComponent(JSON.stringify(this.res)),
								animationType:"zoom-out"
							});
						},
						fail: error => {
							console.log(error);
						},
						complete: () => {}
					});
				}
			},
			returnHome(){
				uni.navigateTo({
					url: '/pages/index/index',
					animationType:"zoom-out"
				});
			},
			deal(){
				if(this.betMoney===0){
					uni.showToast({
						title: '您应该赌一些钱~',
						icon:"none"
					});
				}
				else{
					console.log("开始游戏");
					uni.request({
						url: 'http://192.168.1.104:8080/startGame',
						method: 'GET',
						sslVerify:false,
						data: {
							ante:this.betMoney
						},
						success: res => {
							this.res=res
							console.log(this.res);
							console.log(this.betMoney);
							uni.navigateTo({
								url: '/pages/game/game?bet='+this.betMoney+'&res='+encodeURIComponent(JSON.stringify(this.res)),
								animationType:"zoom-out"
							});
						},
						fail: error => {
							console.log(error);
						},
						complete: () => {}
					});
				}
			}
			,
			clear(){
				this.totalMoney=this.totalMoney+this.betMoney;
				this.betMoney=0;
			}
			,
			allIn(){
				if(this.totalMoney>0){
					this.betMoney=this.totalMoney+this.betMoney;
					this.totalMoney=0;
				}else{
					uni.showModal({
						title: 'You have no money!',
						showCancel: false,
						confirmText: '确认',
						success: res => {},
						fail: () => {},
						complete: () => {}
					});
				}
			},
			tapOne(){
				if(this.totalMoney>=1){
					this.one++;
					this.totalMoney=this.totalMoney-1;
					this.betMoney=this.betMoney+1;
				}else{
					uni.showModal({
						title: 'Your money is not enough!',
						showCancel: false,
						confirmText: '确认',
						success: res => {},
						fail: () => {},
						complete: () => {}
					});
				}
			},
			tapTwo(){
				if(this.totalMoney>=5){
					this.two++;
					this.totalMoney=this.totalMoney-5;
					this.betMoney=this.betMoney+5;
				}else{
					uni.showModal({
						title: 'Your money is not enough!',
						showCancel: false,
						confirmText: '确认',
						success: res => {},
						fail: () => {},
						complete: () => {}
					});
				}
			},
			tapThree(){
				if(this.totalMoney>=25){
					this.three++;
					this.totalMoney=this.totalMoney-25;
					this.betMoney=this.betMoney+25;
				}else{
					uni.showModal({
						title: 'Your money is not enough!',
						showCancel: false,
						confirmText: '确认',
						success: res => {},
						fail: () => {},
						complete: () => {}
					});
				}
			},
			tapFour(){
				if(this.totalMoney>=50){
					this.four++;
					this.totalMoney=this.totalMoney-50;
					this.betMoney=this.betMoney+50;
				}else{
					uni.showModal({
						title: 'Your money is not enough!',
						showCancel: false,
						confirmText: '确认',
						success: res => {},
						fail: () => {},
						complete: () => {}
					});
				}
			}
			,tapFive(){
				if(this.totalMoney>=100){
					this.five++;
					this.totalMoney=this.totalMoney-100;
					this.betMoney=this.betMoney+100;
				}else{
					uni.showModal({
						title: 'Your money is not enough!',
						showCancel: false,
						confirmText: '确认',
						success: res => {},
						fail: () => {},
						complete: () => {}
					});
				}
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
	  padding: 5rpx 50rpx;
	  font-size: 40rpx;
	  font-family: 'Times New Roman', Times, serif;
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
