<script>
	export default {
		onLaunch: function() {
		
			this.bgmMusic = uni.createInnerAudioContext();
			
			this.bgmMusic.autoplay = true;//自动播放
			
			this.bgmMusic.loop = true;//循环播放
			
			this.bgmMusic.src = '../static/bg.mp3';//背景音乐地址
			
			this.bgmMusic.volume = 0.3;//音量
			
			this.bgmMusic.onPlay(function(){
			
			console.log('背景音乐播放中');
			
			});
		
		},

		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
	/*每个页面公共css */
	@import "/common/animate.css";	
	@import "/common/icon.css";
	@import "/common/common.css";
	@import "/common/zcm-main.css";
</style>
