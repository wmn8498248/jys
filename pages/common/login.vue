<template>
	<view class="login-wrap">
		<!-- <image src="../../static/image/icon/login-bg.png" class="login-head-bg" ></image> -->
		<view class="login-logo">
			<image :src="$store.state.logo" class="d-block" mode="aspectFit" style=""></image>
		</view>
		<view class="d-flex-between-center" style="color: white; position: fixed; top: 20px; z-index: 99; width: 100%; padding: 0px 27px;">
			<!-- <text class="d-block font-size-48 my-8 font-weight-bold linear-gradient-text" style="line-height: 66rpx;">{{i18n.login}}</text> -->
			<view class="font-size-32" @click="showLanguage=true">
				<svg  width="20" height="20" viewBox="0 0 48 48" fill="none" xmlns="http://www.w3.org/2000/svg" style="width: 20px; height: 20px; vertical-align: middle; margin-bottom: 2px; margin-right: 5px;">
					<path  d="M24 44.0002C35.0457 44.0002 44 35.0459 44 24.0002C44 12.9545 35.0457 4.00024 24 4.00024C12.9543 4.00024 4 12.9545 4 24.0002C4 35.0459 12.9543 44.0002 24 44.0002Z" stroke="#19161e" stroke-width="3"></path>
					<path  d="M6 30.9856C8.63192 32.041 10.5266 32.041 11.6839 30.9856C13.4199 29.4025 11.9219 24.5978 14.3532 23.2727C16.7844 21.9476 20.4886 27.8214 23.9508 25.8887C27.4129 23.9559 23.6246 18.8023 26.0272 16.713C28.4298 14.6237 31.554 16.98 32.1001 13.4865C32.6462 9.99304 29.5521 11.5082 28.9584 8.20693C28.5625 6.00611 28.5625 4.84884 28.9584 4.73511" stroke="#19161e" stroke-width="3" stroke-linecap="round"></path>
					<path  d="M29.0209 43.3505C27.1468 41.4325 26.4721 39.6497 26.9969 38.0019C27.7841 35.5303 29.0826 35.6764 29.6488 34.1482C30.2149 32.6199 28.6156 30.4433 32.1643 28.5826C34.5301 27.3421 37.783 28.7794 41.9228 32.8944" stroke="#19161e" stroke-width="3" stroke-linecap="round"></path>
				</svg>
				<text style="color: rgb(22, 25, 30);">
					{{i18n.selectLang}}
				</text>
			</view>
			<view class="kefu1" @click="$utils.jump('/pages/kf/kf')">
				<svg  xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#16191e" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
					<path  d="M3 18v-6a9 9 0 0 1 18 0v6"></path>
					<path  d="M21 19a2 2 0 0 1-2 2h-1a2 2 0 0 1-2-2v-3a2 2 0 0 1 2-2h3zM3 19a2 2 0 0 0 2 2h1a2 2 0 0 0 2-2v-3a2 2 0 0 0-2-2H3z"></path>
				</svg>
			</view>
		</view>
		<view class="login-container text-white" style="margin-top: 50%; height: 100%; background: white;">
			<view class="d-flex-between-center" style="justify-content: center; color: white;">
				<text class="d-block font-size-48 my-8 font-weight-bold linear-gradient-text" style="line-height: 34px;"></text>
			</view>
			<view>
				<!-- 邮箱/手机/用户名 -->
				<view class="login-input-group" style="position: relative;">
					<input type="text" v-model="user_string" class="login-input mail_new"
						:placeholder="i18n.email" confirm-type="next" @confirm="passwordFocus=true">
				</view>
				<view class="login-input-group">
					<input type="text" v-model="password" :focus="passwordFocus" @blur="passwordFocus=false" class="login-input password_new" :placeholder="i18n.password" :password="true" @confirm="login" >
				</view>
				
				<view class="mt-20 d-flex-between-center">
					<view class="d-flex align-items-center ">
						<text class="iconfont text-secondary mr-12" :class="rememberPassword ? 'icon-checkbox-ok' : 'icon-checkbox'" @click="rememberPasswordFunc"></text>
						<text>{{i18n.rememberPassword}}</text>
					</view>
					<navigator url="/pages/common/forget">
						{{i18n.forgetPassword}}
					</navigator>
				</view>
				
				<view class="login-btn-group mt-40">
					<button class="warning-button" @click="login">{{i18n.login}}</button>
				</view>
				<view class="d-flex mt-30 mb-16" style="justify-content: center;">
					{{i18n.noaccount + i18n.go}}
					<navigator url="/pages/common/register" class="ml-4 text-warning" style="color: rgb(38, 243, 168);">{{i18n.register}}</navigator>
				</view>
			</view>
		</view>
		
		
		<!-- 选择语言的popup -->
		<u-popup v-model="showLanguage" mode="right" width="100%" :title="i18n.selectLang" :closeable="true" :borderRadius='0'>
			<view class="popup-list">
				<view class="popup-list-item" v-for="item in langs" :key="item.value" :class="{active : item.selected}"
					@click="setLang(item)">
					<text>{{item.name}}</text>
				</view>
			</view>
		</u-popup>
	</view>
</template>
<script>
	import {langs} from "./../setting/data.js"
	export default {
		data() {
			return {
				lang:require("static/image/svg/lang.svg"),
				kefu:require("static/image/svg/kefu.svg"),
				password: uni.getStorageSync('loginPassword') || '',
				user_string: uni.getStorageSync('loginAccount') || '',
				showLanguage:false,
				langs: null,
				passwordFocus: false,
				rememberPassword:uni.getStorageSync('rememberPassword') || false
			};
		},
		onLoad() {
			const _this = this
			uni.setNavigationBarTitle({
				title:_this.$t("common.login")
			})	
		},
		onShow() {
			this.setDefaultLang()
		},
		computed: {
			i18n() {
				return this.$t("common")
			}
		},
		methods: {
			setDefaultLang() {
				let langsData = langs.map(el => {
					el.selected = false
					return el
				})
				const lang = this.$store.state.lang || 'en'
				const has = langsData.findIndex(item => item.value == lang)
				langsData[has].selected = true
				this.langs = langsData
			},
			//设置语言
			setLang(item) {
				let langs = this.langs.map(el => {
					el.selected = false
					if (el.value == item.value) el.selected = true
					return el
				})
				this.langs = langs
				this._i18n.locale = item.value
				this.lang = item
				uni.setStorageSync('lang', item.value)
				this.$store.commit('setLang', item.value)
				
				setTimeout(() => {
					this.showLanguage = false
				}, 200)
			},
			login() {
				this.$u.throttle(async () => {
					const {
						user_string,
						password
					} = this
					if (!user_string) {
						this.$utils.showToast(this.i18n.plsInputUsername)
						return false
					}
					if (!password) {
						this.$utils.showToast(this.i18n.passwordPlaceholder)
						return false
					}
					if(this.$utils.charTest(user_string) || this.$utils.charTest(password) ){
						this.$utils.showToast(this.$t("common.specialChart"))
						return false
					}
					
					try {
						const res = await this.$store.dispatch('login', {
							data: {
								user_string,
								password,
							},
							_this: this
						})
						if (res) {
							//登陆成功，延时1.2s跳转
							this.$utils.showToast(this.i18n.login_success)
							//如果勾选了记住密码，则储存
							if(this.rememberPassword){
								uni.setStorageSync('loginAccount',user_string)
								uni.setStorageSync('loginPassword',password)
							}else{
								uni.removeStorageSync('loginAccount')
								uni.removeStorageSync('loginPassword')
							}
							
							setTimeout(() => {
								uni.reLaunch({
									url: "/pages/index/index"
								})
							}, 1200)
						}
					} catch (e) {
						// 捕获异常
						console.log(e);
					}
				}, 1000)
			},
			rememberPasswordFunc(){
				this.rememberPassword = !this.rememberPassword
				uni.setStorageSync('rememberPassword',this.rememberPassword)
			}
		}
	}
</script>
<style lang="scss" scoped>
	.login-logo {
		position:fixed;
		left:0;
		right:0;
		margin:0 auto
	}
	.login-wrap {
		height: 100vh;
		background-color: $uni-color-333;

		.login-container {
			position: fixed;
			left: 0;
			right: 0;
			padding: 50rpx;
			padding-bottom: 80rpx;
			background-color: $uni-color-black;
			border-radius: 50rpx 50rpx 0 0;
		}
	}
	
	// 弹出层列表
	.popup-list {
		.popup-list-item {
			height: 96rpx;
			line-height: 96rpx;
			padding: 0 30rpx;
			@extend .font-size-32;
			position: relative;
			display: flex;
			align-items: center;
	
			&:before {
				content: "";
				position: absolute;
				left: 30rpx;
				right: 30rpx;
				bottom: 0;
				height: 2rpx;
				background-color: #efefef;
			}
	
			&.active {
				background-color: #f2f6ff;
	
				&:after {
					content: "";
					width: 36rpx;
					height: 20rpx;
					background-image: url('./../../static/image/icon/select.png');
					background-size: cover;
					position: absolute;
					right: 80rpx;
					top: 50%;
					margin-top: -10rpx;
				}
			}
		}
	}
	.mail_new {
	    background-image: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEgAAABICAYAAABV7bNHAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAA4RpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDkuMC1jMDAwIDc5LjE3MWMyN2ZhYiwgMjAyMi8wOC8xNi0yMjozNTo0MSAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wTU09Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9tbS8iIHhtbG5zOnN0UmVmPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvc1R5cGUvUmVzb3VyY2VSZWYjIiB4bWxuczp4bXA9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC8iIHhtcE1NOk9yaWdpbmFsRG9jdW1lbnRJRD0ieG1wLmRpZDoxNDA5MjVjYy1lYjdkLTQwNDEtOGEwMC0yYTliNjljMDU0NGIiIHhtcE1NOkRvY3VtZW50SUQ9InhtcC5kaWQ6MThEOUE3NTA3NjM2MTFFREE0MURBRkI3QjQwQThCOUYiIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6MThEOUE3NEY3NjM2MTFFREE0MURBRkI3QjQwQThCOUYiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIDI0LjAgKFdpbmRvd3MpIj4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6NmM2YWNmYmQtZWIwYS02NDRhLTgwM2UtNTBiZjM2M2I3NDI5IiBzdFJlZjpkb2N1bWVudElEPSJhZG9iZTpkb2NpZDpwaG90b3Nob3A6MTY2OTAxZjMtYTFjMS01NzQyLWI5OTMtNzgyOTE1YTUwNDU5Ii8+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+9WtRUgAAA2NJREFUeNrsnE9IFFEcx2dLooRiLStKOuSeOtQh2bKwWyctC/tjIYt16Fp0SySLCqlLUNCxCIuKNinJTl2Div6hqUG0dugaJRVq7sL2ffQVhsebzdm1nTezvx98kJn31pn3md+89+btaCyfzzsS3rFAFIggESSCRJAIEkEiSARJiCARNP9RZdqZSCQqVkgmk/GdQXHQA4bAJMhHgEm2p4ft85dBrtgABkF9xBJlCdhEjoAW8MFvH7QUPI6gHD3Ws53L/Ao6obqjCul6VDuP+xV0UNu+BVaDWARQ7bijte+ASULMtKLIUSyr9VE1YCJCWaM65++u7WnVN/kZxfQOfCJit5XensXFjGKeMb7uWeiM1H9pkpm0bY8a1eAB+Ar6wCpL2rUS3ACfwGWwMChBx8A+sAKkwHuwP2A56vgj4CiH75OgNShBi7RtlUFpcA/UlllMLY+bNmRydVCCroNXhv3tvIptZZLTxuO1G8pegIGgBH0D2/nQN6OVqclYP7jLW/B/xHJO+Pp5PHfM8Lx2gF9BrgflwHmwBbwzlB8Co2DvPMvZA8bAYUOZOo8kzysX5CjmDrV8sBWc4Sxcz6aH4DaveqlZo37PI0PWZHl8dR7DNgzzjuEEz/HqDRnKO9hXFDuqtPLzHR4XKMnjZ22ZBxXKJq+TXcNOs4/Pd3OJGtYf4Of9XBQrBc0l3VPMhl3/+D0trJcylA0XuK2tF6R3mBcMHeZaLlbdNGRTnPsHWc80MCQ9BoZQCZodck+DRmaDHp2chTdzu5n1Og11R5g1pqlFaAXNxhvQAHoN2VQHnoDn/FlnyJpefv5tWB5Wi82mbrCN8yM9Gg37Rlm/uxxZY8tyx2tmw8UCk7kcyxtYP3TLHaXGb9AFmjgrdscY93exXiBR5dgRL5klp8BO8BRccv6uEwcatghyKOMssSaKFlTM+m4YQ9akRZAIEkEiSASJIBEkgiREkAgSQXYJ0hex4hFru96eab+C9PeGrzr2vP9Taqh2XNP2fTRVLLTccR9sdG2nHPN3U1GJtN8MugIyFdLVZHiH+BL0E+wG4xGX85nt/FHMKKb6IbVWrL7aVV/zTkVEyhTbo9q12fH4Ow0VhV4kr8jQXySPyf/ukImiCBJBIkgEiSARJIIkRJAIEkFljz8CDABIgOFmBNhZ0gAAAABJRU5ErkJggg==);
	    background-size: 25px 25px;
	    background-position: 5px 6px;
	    background-repeat: no-repeat;
	    padding-left: 38px;
	}
	.password_new {
	    background-image: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEgAAABICAYAAABV7bNHAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAA4RpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDkuMC1jMDAwIDc5LjE3MWMyN2ZhYiwgMjAyMi8wOC8xNi0yMjozNTo0MSAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wTU09Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9tbS8iIHhtbG5zOnN0UmVmPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvc1R5cGUvUmVzb3VyY2VSZWYjIiB4bWxuczp4bXA9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC8iIHhtcE1NOk9yaWdpbmFsRG9jdW1lbnRJRD0ieG1wLmRpZDoxNDA5MjVjYy1lYjdkLTQwNDEtOGEwMC0yYTliNjljMDU0NGIiIHhtcE1NOkRvY3VtZW50SUQ9InhtcC5kaWQ6MjY1Rjg2RTk3NjM2MTFFRDk5QjFEMTAyODI0RTJCQjYiIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6MjY1Rjg2RTg3NjM2MTFFRDk5QjFEMTAyODI0RTJCQjYiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIDI0LjAgKFdpbmRvd3MpIj4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6NmM2YWNmYmQtZWIwYS02NDRhLTgwM2UtNTBiZjM2M2I3NDI5IiBzdFJlZjpkb2N1bWVudElEPSJhZG9iZTpkb2NpZDpwaG90b3Nob3A6MTY2OTAxZjMtYTFjMS01NzQyLWI5OTMtNzgyOTE1YTUwNDU5Ii8+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+diXZIwAAA1BJREFUeNrsnE1oU0EQx19MG6jWKoKCYi1tpIJUD/WgFhHB9OIHCBVBlNaTB7EKiqAHFfRSFFQQD4Ig1tqbeNCD6MEKSkUPCnrxIyoK7UUUpPiFJv6HTOQ5Js1Ls6aZlxn4HfblveXt/+3OzuxuG0mn055ZfptiEphAJpAJZAJVrtW4rCyyZnRSG5MemlvZAhWjJegAG8EKsAjMAjHwA3wEL8AwuAHuU/sn5UVdxkEBehAN6W5wkEUJas9BH+gHqXL2oHL6oMXgIbhYpDge30/PPQJtYXTSm1mcZSXW0871bFHppPPYVnAZRMV1Giq3wHVu9Aj4yvfNYx+1CXSKD1kHBsE07lWqfdBqcJudr99ugr3siAtZG/uf9eL6L5AAQ1p90HR2qjHRqH1gXUBxyJ7xbLeHn89alHvmTK0+iGaqJv8kA3aA0xOYsun+s6BHPDsfHNIoUD3YLa6dAQMl1nsFnBDXdoEZ2gTqAg2+8gdwxFHdx8Co+Bhd2gRK5PjyY47q/gIuiGud2gRaKsrXHNd/R5SXaBNotii/clz/G1Geo02gqaI85rj+T6Ic055qqDWtAsk4qlZjHOS3b47r/1xgSFe8QDIx/a51iI2bzcfj8eJqa7z3J28tw7tHSn5fWDKZdLbcUcd5zzbOsWQv8Vrer6rUjkBJ7jtOdfo42Cy9B4kxTssWHUpHCn3MZnCYo/xEUJGC+qADisWRtpLb49RJbw9ZeNPteog1S3+oUBS/N25yLZB0yK+V96CopRqWi5lAJpAJZAKF32oq5D0WgJ2gFbwE5zl3MoG8zPkg2or2723RLuoGcLfahxgFbIPevxt/tOBG28q11S5Qe440JmuNYHm1C1Rf4PeGahfoqZc5k5jLfoIn1S4Q7dmfy/MbzWQjFgdlFq/oQEJ2hY8W+E95mQNWFih6mfXio+ASl6+C/d7fh6UskrZUwwQygf63pX0+yZLVHEanxlKcYphAOewx6LUhZj7IBDKBzEwgE6jc0zwFb/797Bbl7U65FogObi/0lZPKBXrreogNhGzk9LsW6CR4EBJxhrk9TgWi1b614DgPt5QyUVL83vT+gc8nko37N6sTOVarzQodA47YP1iyOMgEMoFMIBPIBAqr/RZgAJWcmpGmD9QOAAAAAElFTkSuQmCC);
	    background-size: 25px 25px;
	    background-position: 5px 6px;
	    background-repeat: no-repeat;
	    padding-left: 38px;
	}
</style>
