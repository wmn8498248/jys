<template>
	<view class="text-white">
		<u-navbar backIconColor="black" :borderBottom='false' :isFixed="false" :background="{background:'rgb(242, 246, 247)'}" title-color="black" :title="$t('setting.grxx')"></u-navbar>
		<view class="list">
			<view class="item" @click="showAvatarPopup = true">
				<text>{{$t("setting.avatar")}}</text>
				<view class="right">
					<image :src="$store.state.user.head_portrait || retImageUrl" class="avatar" mode="aspectFill"></image>
					<text class="iconfont icon-gengduo1 ml-20"></text>
				</view>
			</view>
			<view class="item" @click="showPopupFunc('nickname')">
				<text>{{$t("setting.nickname")}}</text>
				<view class="right">
					<text>{{$store.state.user.nickname || i18n.nonickname }}</text>
					<text class="iconfont icon-gengduo1 ml-20"></text>
				</view>
			</view>
			<view class="item">
				<text>{{$t("setting.email")}}</text>
				<view class="right">
					<text>{{$store.state.user.account_number }}</text>
				</view>
			</view>
			<view class="item">
				<text>{{$t("setting.score")}}</text>
				<view class="right">
					<text>{{$store.state.user.score }}</text>
				</view>
			</view>
			<!-- <view class="item" @click="showPopupFunc('signature')">
				<text>{{$t("setting.signature")}}</text>
				<view class="right">
					<text>{{signature.length > 8 ? signature.slice(0,8) + '...' : signature}}</text>
					<text class="iconfont icon-gengduo1 ml-20"></text>
				</view>
			</view> -->
			<!-- <navigator class="item" url="/pages/setting/real_mark">
				<text>{{$t("setting.authentication")}}</text>
				<view class="right">
					<text class="iconfont icon-gengduo1 ml-20"></text>
				</view>
			</navigator> -->
			<!-- <view class="item">
				<text>{{$t("setting.authentication2")}}</text>
				<view class="right">
					<text v-if="real.advanced_review_status == 0" class="text-error">{{i18n.noaudit}}</text>
					<text v-else-if="real.advanced_review_status == 1" class="text-warning">{{i18n.auditing}}</text>
					<text v-else-if="real.advanced_review_status == 2" class="text-success">{{i18n.hasaudit}}</text>
					<text class="iconfont icon-gengduo1 ml-20"></text>
				</view>
			</view> -->
			<button class="primary-button mt-200" @click="logout">{{$t("common.logout")}}</button>
		</view>
		<u-popup v-model="showAvatarPopup" mode="bottom" length="60%" >
			<view class="m-30" style="padding-bottom: 300rpx;">
				<view class="d-flex d-flex-between-center">
					<image :src="'/static/image/avatar/'+item+'.png'" :class="'d-flex d-flex-between-center justify-content-center '+(item==active?'image_check':'')" @click="active=item" v-for="item in avatars.slice(0,5)" style="width: 126rpx; height: 126rpx;"></image>
				</view>
				<view class="d-flex d-flex-between-center mt-30">
					<image :src="'/static/image/avatar/'+item+'.png'" :class="'d-flex d-flex-between-center justify-content-center '+(item==active?'image_check':'')" @click="active=item" v-for="item in avatars.slice(5,10)" style="width: 126rpx; height: 126rpx;"></image>
				</view>
				<view class="d-flex d-flex-between-center mt-30">
					<image :src="'/static/image/avatar/'+item+'.png'" :class="'d-flex d-flex-between-center justify-content-center '+(item==active?'image_check':'')" @click="active=item" v-for="item in avatars.slice(10,15)" style="width: 126rpx; height: 126rpx;"></image>
				</view>
				<view class="d-flex d-flex-between-center mt-30">
					<image :src="'/static/image/avatar/'+item+'.png'" :class="'d-flex d-flex-between-center justify-content-center '+(item==active?'image_check':'')" @click="active=item" v-for="item in avatars.slice(15,20)" style="width: 126rpx; height: 126rpx;"></image>
				</view>
				<view class="d-flex d-flex-between-center mt-30">
					<image :src="'/static/image/avatar/'+item+'.png'" :class="'d-flex d-flex-between-center justify-content-center '+(item==active?'image_check':'')" @click="active=item" v-for="item in avatars.slice(20,25)" style="width: 126rpx; height: 126rpx;"></image>
				</view>
				<view class="d-flex d-flex-between-center mt-30">
					<image :src="'/static/image/avatar/'+item+'.png'" :class="'d-flex d-flex-between-center justify-content-center '+(item==active?'image_check':'')" @click="active=item" v-for="item in avatars.slice(25,30)" style="width: 126rpx; height: 126rpx;"></image>
				</view>
				<view class="d-flex d-flex-between-center mt-30" style="position: fixed; bottom: 0px; left: 0px; right: 0px; background-color: white; padding: 20rpx 0px;">
					<button style="width: 42%; background-color: rgb(199, 199, 199); color: rgb(2, 62, 223);" @click="showAvatarPopup = false">{{$t("common.cancel")}}</button>
					<button style="width: 42%; background-color: rgb(2, 62, 223); color: white;" @click="uploadAvatar">{{$t("common.confirm")}}</button>
				</view>
			</view>
		</u-popup>
		
		<u-popup v-model="showPopup" mode="bottom" length="28%" >
			<view class="m-30">
				<view class="d-flex-between-center font-size-32">
					<text>{{showOptions.title}}</text>
					<text class="opacity-50" @click="showPopup = false">{{$t("common.cancel")}}</text>
				</view>
				<view class="mt-30">
					<input type="text" class="input" v-model="value" :placeholder="showOptions.placeholder">
					<text class="d-block font-size-22 opacity-50 pl-26 mt-20">{{showOptions.desc}}</text>
				</view>
				<button class="primary-button mt-40" @click="edit">{{$t("common.confirm")}}</button>
			</view>
		</u-popup>
		
		<!-- <u-action-sheet :list="photoTypes" v-model="showPhotoTypeSelect" :title="$t('setting.xxtx')" @click="showPhotoFunc"></u-action-sheet> -->
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				avatars:[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30],
				signature:'signature',
				active: '',
				showPopup:false,
				showAvatarPopup:false,
				showOptions:{},
				value:'',
				//展示选择相册还是相机
				showPhotoTypeSelect:false,
				real:{},
				showPopName:null
			};
		},
		onShow() {
			this.getWalletList()
		},
		methods:{
			
			getWalletList() {
				this.$u.api.wallet.getWalletList().then(res => {
					this.originalList = res.message
				})
			},
			//退出登陆
			async logout(){
				const ret = await this.$utils.showModal(this.$t("common.hint"),this.$t("setting.confirmLogout"))
				if(!ret) return
				this.$u.api.user.logout().then(res=>{
					this.$utils.showToast(res.message)
					this.$store.commit('deleteUser')
					setTimeout(()=>{
						uni.reLaunch({
							url:'/pages/index/index'
						})
					},1200)
				})
			},
			showPopupFunc(name){
				this.showPopName = name
				const options = this.$t("setting.grxxOptions")
				this.showOptions = options[name]
				this.showPopup = true
			},
			showPhotoFunc(index){
				const sourceType = index == 0 ? ['camera'] : ['album']
				uni.chooseImage({
				    count: 1, //默认9
				    sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
				    sourceType, //从相册选择
				    success: function (res) {
				        console.log(JSON.stringify(res.tempFilePaths));
				    }
				});
			},
			async uploadAvatar(){
				const ret = '/static/image/avatar/'+this.active+'.png'
				
				this.$u.api.setting.uploadAvatar(ret).then(res=>{
					let user = this.$u.deepClone(this.$store.state.user)
					user.head_portrait = ret
					this.$store.commit('refreshUser',user)
					this.$utils.showToast(this.$t("common.success"))
					this.showAvatarPopup = false
				})
			},
			//修改昵称或者个性签名
			edit(){
				const {showPopName,value,showOptions} = this
				if(showPopName == 'nickname'){
					if(!value.length){
						return this.$utils.showToast(showOptions.placeholder)
					}
					if(this.$utils.charTest(value)){
						return this.$utils.showToast(this.$t("common.specialChart"))
					}
					if(value.length > 10){
						return this.$utils.showToast(this.$t("common.toLong"))
					}
					
					this.$u.api.setting.editNickname(value).then(res=>{
						let user = this.$u.deepClone(this.$store.state.user)
						user.nickname = value
						this.$store.commit('refreshUser',user)
						
						this.$utils.showToast(this.$t("common.success"))
						this.showPopup = false
					})
				}
			}
		},
		computed:{
			i18n(){
				return this.$t("setting")
			},
			photoTypes(){
				const lang = this.$t("setting")
				return [
					// #ifdef APP-PLUS
					{
						text:lang.photograph,
					},
					// #endif
					{
						text:lang.selectFromAlbum,
					}
				]
			}
		}
	}
</script>

<style lang="scss" scoped>
.list{
	margin: 0 30rpx;
	.item{
		@extend .d-flex-between-center;
		height: 110rpx;
		line-height: 110rpx;
		font-size: 32rpx;
		.right{
			@extend .d-flex-between-center;
			
			.avatar{
				height: 68rpx;
				width: 68rpx;
				border-radius: 50%;
			}
			
			text{
				font-size: 28rpx !important;
				opacity: .5;
			}
		}
	}
}
.image_check {
    border-radius: 50%;
    border: 6rpx solid #023edf;
}
</style>
