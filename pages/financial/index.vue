<template>
	<view class="home">
		<scroll-view>
			<view style="height: 1500rpx;overflow-y: auto;">
				<view class=" z-index-10 mb-50">
					<view style="position: relative;">
						<view style="position: absolute; bottom: -20rpx; z-index: 99; left: 30rpx;">
							<view class="font-size-52" style="color: rgb(55, 210, 1);">
								{{i18n.efficiency_safe_robust}}
							</view>
						</view>
						<image src="/static/img/jr_new.png" style="width: 100%; height: 550rpx;"></image>
					</view>
				</view>
				<!-- <view class="px-30 text-white pt-30">
					<text class="font-size-40 text-black font-weight-bold">{{i18n.financial}}</text> 
				</view>
				<view class="mx-30 mt-26 z-index-10 mb-70">
					<view class="d-flex align-items-center justify-content-between ">
						<view class="bg-white d-flex px-40 py-30 border-radius-10 align-items-center w-49" v-for="item in menu.slice(0,2)" @click="homeNavJump(item.url,item.open_type)">
							<image :src="`../../static/image/icon/${item.icon}.png`" class="d-block" style="width: 50rpx;height: 50rpx;" mode=""></image>
							<text class="d-block font-size-32 text-black ml-20 font-weight-bold">{{item.name}}</text>
						</view>
					</view>
					<view class="d-flex align-items-center justify-content-center bg-white border-radius-10 py-20 mt-10" v-for="item in menu.slice(2,3)" @click="homeNavJump(item.url,item.open_type)">
						<image :src="`../../static/image/icon/${item.icon}.png`" class="d-block" style="width: 50rpx;height: 50rpx;" mode=""></image>
						<text class="d-block font-size-32 text-black ml-20 font-weight-bold">{{item.name}}</text>
					</view>
				</view> -->
				
				<!-- 投资理财 -->
				<view class="mb-50" v-if="invest.length">
					<view class=" px-30">
						<view class="d-flex-between-center text-black">
							<text class="d-block font-size-32 font-weight-bold">{{i18n.invest}}</text>
							<view class="more"><u-icon name="arrow-right" size="28" @click="$utils.jump('/pages/invest/invest')"></u-icon></view>
						</view>
						<view class="d-flex align-items-center mt-20 font-size-32">
							<text class="mr-30 pb-20 nav-item text-black" @click="activeInvest = 0" :class="activeInvest == 0 ? 'active' : 'opacity-75'">BTC {{i18n.section}}</text>
							<text class="mr-30 pb-20 nav-item text-black" @click="activeInvest = 1" :class="activeInvest == 1 ? 'active' : 'opacity-75'">ETH {{i18n.section}}</text>
						</view>
					</view>
					<view class="mx-30 my-20 bg-white p-30 border-radius-20 bg-white" v-for="item in invest[activeInvest].slice(0,3)" @click="$utils.jump('/pages/invest/purchase?id='+item.id)">
						<view class="d-flex align-items-center">
							<view class="border-radius-50per d-flex align-items-center justify-content-center" style="width: 50rpx;height: 50rpx;" :style="{backgroundColor:$utils.getCurrencyColor(item.currency_name)}">
								<text class="iconfont font-size-40 text-white" :class="`icon-${item.currency_name}`" ></text>
							</view>
							<text class="font-weight-bold ml-12 font-size-32 text-black">{{item.name}}</text>
						</view>
						<view class="d-grid-columns-3 mt-20">
							<view class="">
								<text class="d-block opacity-50 font-size-24 mb-10" style="color: #aaa;">{{i18n.apr}}</text>
								<text class="d-block font-size-40 font-weight-bold" :style="{color:$upColor}">{{item.rate}}%</text>
							</view>
							<view class="text-center">
								<text class="d-block opacity-50 font-size-24 mb-10" style="color: #aaa;">{{i18n.linkedReferencePrice}}</text>
								<text class="d-block font-size-40 font-weight-bold text-white opacity-80" >{{item.exercise_price}}</text>
							</view>
							<view class="text-right">
								<text class="d-block opacity-50 font-size-24 mb-10" style="color: #aaa;">{{i18n.holdingDays}}</text>
								<text class="d-block font-size-40 font-weight-bold text-white opacity-80">{{item.remainDays + i18n.day}}</text>
							</view>
						</view>
					</view>
				</view>
				
				<!-- 锁仓挖矿 -->
				<view class="mb-50" v-if="lockming.length">
					<view class=" px-30 d-flex-between-center text-black">
						<text class="d-block font-size-32 font-weight-bold">{{i18n.lockming}}</text>
						<view class="more"><u-icon name="arrow-right" size="28" @click="$utils.jump('/pages/lockming/welcome')"></u-icon></view>
					</view>
					<view class="m-30 mt-20 high-quality-project">
						<swiper style="height: 390rpx;" :acceleration="true" :display-multiple-items="2" :indicator-dots="false"
							:autoplay="false" :circular="false">
							<swiper-item v-for="(item,index) in lockming.slice(0,5)">
								<view class="high-quality-project-item bg-white text-white" @click="jump2Lockming(item)">
									<text class="hige-quality-project-sskc">{{item.day + i18n.day}}</text>
									<view class="mx-auto border-radius-50per d-flex align-items-center justify-content-center" style="width: 80rpx;height: 80rpx;" :style="{backgroundColor:$utils.getCurrencyColor(item.currency_name)}">
										<text class="iconfont font-size-48 text-white" :class="`icon-${item.currency_name}`" ></text>
									</view>
									
									<text class="d-block font-size-32 mt-20">{{item.currency_name + ' ' +i18n.mining}}</text>
									<text
										class="d-block font-size-28 opacity-50 mt-4">{{Number(item.save_min) + ' ' + i18n.minimum}}</text>
									<text
										class="d-block font-size-40 text-error mt-30 font-weight-bold">{{Number(item.interest_rate)+'%'}}</text>
									<text class="d-block font-size-28 mt-2">{{i18n.dailyReturnRate}}</text>
								</view>
							</swiper-item>
						</swiper>
					</view>
				</view>
				
				<!-- ieo -->
				<view class="mb-50" v-if="ieo.length">
					<view class=" px-30 d-flex-between-center text-black">
						<text class="d-block font-size-32 font-weight-bold">{{i18n.ieo}}</text>
						<view class="more"><u-icon name="arrow-right" size="28" @click="$utils.jump('/pages/ieo/ieo')"></u-icon></view>
					</view>
					<view class="mx-30 my-20 p-30 box-shadow border-radius-20 mb-20 bg-white text-white" v-for="item in ieo.slice(0,3)" >
						<view class="d-flex-between-center">
							<text class="d-block font-size-30 text-black font-weight-bold">{{item.title}}</text>
							<u-icon name="arrow-rightward" size="40" v-if="item.time_status == 2"  @click="$utils.jump('/pages/ieo/subscribe?project_id=' + item.id)"></u-icon>
						</view>
						<view class="d-flex-between-center mt-20">
							<view class="d-flex align-items-baseline">
								<text class="opacity-50 font-size-24">{{i18n.lockPeriod}}:</text>
								<text class="font-weight-bold font-weight-bold ml-12">{{item.day + i18n.day}}</text>
							</view>
							<text class="text-success font-size-24" v-if="item.time_status == 2">{{i18n.ing}}</text>
							<text class="text-error font-size-24" v-else-if="item.time_status == 3">{{i18n.done}}</text>
						</view>
						<view class="d-grid-columns-2 mt-20">
							<view class="d-flex align-items-baseline">
								<text class="opacity-50 font-size-24">{{i18n.subscribed}}({{item.currency_name}}):</text>
								<text class="font-weight-bold font-weight-bold ml-12">{{parseFloat(item.total_sell)}}</text>
							</view>
							<view class="d-flex align-items-baseline">
								<text class="opacity-50 font-size-24">{{i18n.total}}({{item.currency_name}}):</text>
								<text class="font-weight-bold font-weight-bold ml-12">{{parseFloat(item.amount)}}</text>
							</view>
						</view>
						<view class="d-grid align-items-baseline" style="grid-template-columns:2fr 1fr">
							<u-line-progress :percent="Number(item.percentage)" ac inactive-color="#666" :show-percent="false" class="mt-20" v-if="item.percentage > 0"></u-line-progress>
							<u-line-progress :percent="0" class="mt-20" inactive-color="#666" :show-percent="false" v-else></u-line-progress>
							<view class="d-flex align-items-baseline justify-content-end">
								<text class="opacity-50 font-size-24">{{i18n.remaining}}:</text>
								<text class="font-weight-bold font-weight-bold ml-12">{{parseFloat(item.percentage)}}%</text>
							</view>
						</view>
					</view>
				</view>
			</view>
		</scroll-view>
		<view class="tab-bar">
			<view :class="'tab-bar-item' + (navActive==index?' router-link-active':'') " v-for="(item,index) in nav" @click="toNav(index)">
				<view :class="'tab_img' + (navActive==index?' tab_img_select':'') ">
					<image :src="'/static/image/nav/nav-'+(index+1)+(navActive==index?'-active':'')+'.svg'"></image>
				</view>
				<view class="tab_text" :style="navActive==index?'color: rgb(39, 109, 255)':''">
					{{item}}
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				navActive:3,
				menu: [],
				ieo:[],
				lockming:[],
				invest:[],
				activeInvest:0
			};
		},
		onLoad() {
			//获取首页菜单
			this.getMenu()
			
			const _this = this
			uni.setNavigationBarTitle({
				title:_this.$t("home.financial")
			})
		},
		methods: {
			toNav(index){
				console.log(0,index)
				if(index==0){
					this.$utils.jump("/",'switchTab')
				}else if(index==1){
					this.$utils.jump("/pages/market/market",'switchTab')
				}else if(index==2){
					this.$utils.jump("/pages/transaction/index",'switchTab')
				}else if(index==3){
					this.$utils.jump("/pages/financial/index",'switchTab')
				}else if(index==4){
					this.$utils.jump("/pages/fund/assets",'switchTab')
				}
			},
			//获取首页菜单
			getMenu() {
				const {
					i18n
				} = this
				this.$u.api.index.getMenu().then(res => {
					this.menu = res.message.filter(item => item.is_financial).map(item=>{
						item.name = i18n[item.title]
						return item
					})
					
					this.menu.forEach(item => {
						if(item.title == 'ieo'){
							this.getIEOProject()
						}else if(item.title == 'lockming'){
							this.getLockming()
						}else if(item.title == 'invest'){
							this.getInvest()
						}
					})
				})
			},
			// 获取ieo认购
			getIEOProject(){
				this.$u.api.ieo.getIEOProject(1,10).then(res=>{
					this.ieo = res.message.list
				})
			},
			// 获取锁仓挖矿
			getLockming(){
				this.$u.api.lockming.getLockming().then(res=>{
					this.lockming = res.message
				})
			},
			// 获取投资理财
			async getInvest(){
				let list_BTC = await this.$u.api.invest.getList(1)
				let list_ETH = await this.$u.api.invest.getList(2)
				
				list_BTC = list_BTC.message.dual_currencys.map(item => {
					item.remainDays = this.datedifference(item.end_time,item.start_time)
					return item
				})
				
				list_ETH = list_ETH.message.dual_currencys.map(item => {
					item.remainDays = this.datedifference(item.end_time,item.start_time)
					return item
				})
				
				this.invest = [list_BTC, list_ETH]
				
			},
			datedifference(sDate1, sDate2) {    //sDate1和sDate2是2006-12-18格式
				var dateSpan,
					tempDate,
					iDays;
				sDate1 = Date.parse(sDate1);
				sDate2 = Date.parse(sDate2);
				dateSpan = sDate2 - sDate1;
				dateSpan = Math.abs(dateSpan);
				iDays = Math.floor(dateSpan / (24 * 3600 * 1000));
				return iDays
			},
			//跳转至锁仓挖矿详情
			jump2Lockming(item) {
				uni.setStorageSync('lockming', item)
				uni.navigateTo({
					url: '/pages/lockming/welcome'
				})
			},
			//点击首页菜单的跳转
			homeNavJump(url,openType){
				if(url){
					this.$utils.jump(url,openType)
				}else{
					this.$utils.showToast(this.$t("common.functionLoading"))
				}
			}
		},
		computed: {
			i18n() {
				return this.$t("financial")
			},
			nav() {
				return this.$t("nav")
			},
		}
	}
</script>

<style lang="scss" scoped>
	.home {
		overflow: hidden;
		padding-top: var(--status-bar-height);
		position: relative;
		min-height: 100vh;
		z-index: 10;
		&::after {
			content: "";
			position: absolute;
			left: 0;
			right: 0;
			top: 0;
			bottom:0;
			background-image: url('@/static/image/icon/login-bg.png');
			background-size: contain;
			background-position: 100% top;
			background-repeat: no-repeat;
			z-index: -1;
		}
	}
	
	.nav-item{
		position: relative;
		&.active::after{
			display: block;
			position: absolute;
			content: "";
			left: 30%;
			right: 30%;
			height: 4rpx;
			background-color: $uni-color-warning;
			bottom: -2rpx;
		}
	}
	
	.high-quality-project {
		.high-quality-project-item {
			border-radius: 20rpx;
			width: 96%;
			text-align: center;
			padding-top: 40rpx;
			height: 390rpx;
	
			.hige-quality-project-sskc {
				background-image: linear-gradient(to right, #00d789, #00e1cc);
				font-size: 24rpx;
				color: #fff;
				height: 40rpx;
				line-height: 40rpx;
				padding: 0 18rpx;
				border-radius: 10rpx 0 10rpx 0;
				display: block;
				position: absolute;
				left: 0;
				top: 0;
			}
		}
	}
	
	.more{
		width: 40rpx;
		height: 40rpx;
		border-radius: 50%;
		display: flex;
		align-items: center;
		justify-content: center;
	}
</style>
