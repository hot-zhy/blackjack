<template>
	<view>
		<view class="" style="display: flex;margin-top: 20rpx;margin-left: 10rpx;">
			<image src="../../static/images/3.webp" mode="widthFix" style="width: 100rpx;"></image>
			<view class="" style="margin-top: 25rpx;font-weight: bold;font-size: 30rpx;">{{pokerCnt}}</view>
		</view>
		
		<view class="" style="display: flex;flex-direction: row;justify-content: center;font-size: 70rpx;
		font-family: cursive;color: maroon;">Dealer:{{res.data.bookMaker.points}}</view>
		<view class="" style="margin-top: 30rpx;display: flex;flex-direction: row;justify-content: center">
			<view class="" v-for="(item,index) in res.data.bookMaker.pokers" style="display: flex;">
				<image :src="item.imgUrl" mode="widthFix" style="width: 120rpx;" v-if="item.bright"></image>
				<image src="../../static/images/back.png" mode="widthFix" v-if="!item.bright" style="width: 120rpx;"></image>
			</view>
		</view>
		<view style="margin-top: 30rpx;display: flex;font-family: cursive;color: darkmagenta;font-size: 50rpx;flex-direction: row;justify-content: center">
			<view @click="double">
				<image src="../../static/images/double.png" style="width: 150rpx;height: 150rpx;" mode=""></image>
			</view>
			<view style="margin-left: 10rpx;margin-top: 40rpx;">bet:{{betMoney}}</view>
		</view>
		<view class="" style="margin-top: 30rpx;display: flex;flex-direction: row;justify-content: center">
			<view class="" v-for="(item,index) in res.data.player.pokers" style="display: flex;">
				<image :src="item.imgUrl" mode="widthFix" style="width: 120rpx;"></image>
			</view>
		</view>
		<view class="" style="margin-top: 30rpx;height: 100rpx;display: flex;flex-direction: row;justify-content: center;font-size: 70rpx;
		font-family: cursive;color: maroon;">Player:{{res.data.player.points}}</view>
		<view class="" v-if="isSplit">
			<button class="button" style="width: 40%;margin-top:10rpx;" @click="hit">Hit</button>
			<button class="button" style="width: 40%;margin-top: 20rpx;" @click="stand">Stand</button>
			<button class="button" style="width: 40%;margin-top: 20rpx;" @click="split">Split</button>
		</view>
		<view class="" v-if="!isSplit">
			<button class="button" style="width: 40%;margin-top:30rpx;" @click="hit">Hit</button>
			<button class="button" style="width: 40%;margin-top: 40rpx;" @click="stand">Stand</button>
		</view>
		
	</view>
</template>

<script>
	export default {
		components:{
			
		},
		data() {
			return {
				betMoney:0,
				pokerCnt:48,
				res:{
					
				},
				isSplit:false
				
			}
		},
		onLoad(options) {
			this.res=JSON.parse(decodeURIComponent(options.res));
			this.betMoney=options.bet
			this.pokerCnt=this.res.data.pokerCnt
			console.log(this.res.data.player.pokers);
			if(this.res.data.player.blackJack){
				let bet=uni.getStorageSync('betMoney')+this.betMoney
				uni.setStorageSync('betMoney',bet)
				uni.showModal({
					title: '您获得了黑杰克，赢啦！',
					content: '',
					showCancel: false,
					cancelText: '',
					confirmText: '返回首页',
					success: res => {
						uni.navigateTo({
							url: '/pages/prepare/prepare',
							animationType:"zoom-out"
						});
					},
					fail: () => {},
					complete: () => {}
				},2000);
			}else if(this.res.data.bookMaker.blackJack){
				let bet=uni.getStorageSync('betMoney')-this.betMoney
				uni.setStorageSync('betMoney',bet)
				uni.showModal({
					title: '庄家获得了黑杰克，您输啦~',
					content: '',
					showCancel: false,
					cancelText: '取消',
					confirmText: '返回',
					success: res => {
						uni.navigateTo({
							url: '/pages/prepare/prepare',
							animationType:"zoom-out"
						});
						
					},
					fail: () => {},
					complete: () => {}
				});
			}
		},
		methods: {
			split(){
				if(2*this.betMoney>uni.getStorageSync('betMoney')){
					uni.showToast({
						title:'钱不够啦',
						icon:"none"
					})
				}else{
					//分牌
					uni.request({
						url: 'http://192.168.1.104:8080/split',
						method: 'GET',
						data: {},
						success: res => {
							console.log(res);
							this.res=res
							//赌注加倍
							this.betMoney=2*this.betMoney
							this.pokerCnt=res.data.pokerCnt
							
						},
						fail: () => {},
						complete: () => {}
					});
				}
			},
			double(){
				if(2*this.betMoney>uni.getStorageSync('betMoney')){
					uni.showToast({
						title: '钱不够啦！',
						icon:"none"
					});
				}else{
					this.betMoney=2*this.betMoney
				}
			},
			stand(){
				uni.request({
					url: 'http://192.168.1.104:8080/stand',
					method: 'GET',
					data: {},
					success: res => {
						console.log(res);
						this.res=res
						this.pokerCnt=res.data.pokerCnt
						if(res.data.bookMaker.burst){
							//庄家爆掉，玩家赢了
							uni.showModal({
								title: '庄家爆掉了，您赢啦！',
								content: '',
								showCancel: false,
								cancelText: '取消',
								confirmText: '返回',
								success: res => {
									uni.navigateTo({
										url: '/pages/prepare/prepare',
										animationType:"zoom-out"
									});
								},
								fail: () => {},
								complete: () => {}
							});
						}else{
							//庄家没有爆掉，持续拿牌直至点数不小于17，开始比较大小
							if(res.data.player.points>res.data.bookMaker.points){
								uni.showModal({
									title: '玩家赢啦！',
									content: '',
									showCancel: false,
									cancelText: '取消',
									confirmText: '返回',
									success: res => {
										uni.navigateTo({
											url: '/pages/prepare/prepare',
											animationType:"zoom-out"
										});
									},
									fail: () => {},
									complete: () => {}
								});
							}else if(res.data.player.points<res.data.bookMaker.points){
								let bet=uni.getStorageSync('betMoney')-this.betMoney
								uni.setStorageSync('betMoney',bet)
								uni.showModal({
									title: '庄家赢啦',
									content: '',
									showCancel: false,
									cancelText: '取消',
									confirmText: '返回',
									success: res => {
										uni.navigateTo({
											url: '/pages/prepare/prepare',
											animationType:"zoom-out"
										});
									},
									fail: () => {},
									complete: () => {}
								});
							}else if(res.data.player.points===res.data.bookMaker.points){
								uni.showModal({
									title: '平局啦！',
									content: '',
									showCancel: false,
									cancelText: '取消',
									confirmText: '返回',
									success: res => {
										uni.navigateTo({
											url: '/pages/prepare/prepare',
											animationType:"zoom-out"
										});
									},
									fail: () => {},
									complete: () => {}
								});
							}
						}
					},
					fail: () => {},
					complete: () => {}
				});
			},
			hit(){
				uni.request({
					url: 'http://192.168.1.104:8080/hit',
					method: 'GET',
					data: {
						
					},
					success: res => {
						this.res=res
						this.pokerCnt=res.data.pokerCnt
						// hit完之后判断有没有两张相同的牌,如果有就可以选择split
						let pokers=res.data.player.pokers
						for(let i=0;i<pokers.length;i++){
							for(let j=i+1;j<pokers.length;j++){
								if(pokers[i].point===pokers[j].point){
									this.isSplit=true
									break
								}
							}
						}
						console.log(pokers);
						
						if(res.data.player.burst){
							let bet=uni.getStorageSync('betMoney')-this.betMoney
							uni.setStorageSync('betMoney',bet)
							//玩家burst，庄家赢了
							uni.showModal({
								title: '您爆掉了！庄家赢啦~',
								content: '',
								showCancel: false,
								cancelText: '取消',
								confirmText: '返回',
								success: res => {
									uni.navigateTo({
										url: '/pages/prepare/prepare',
										animationType:"zoom-out"
									});
								},
								fail: () => {},
								complete: () => {}
							});
						}else{
							// 玩家没有burst,继续选择
						}
					},
					fail: () => {},
					complete: () => {}
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
