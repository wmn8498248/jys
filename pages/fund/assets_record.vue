<template>
	<view class="pb-50">
		<u-navbar backIconColor="black" :borderBottom='false' :isFixed="false" :background="{background:'rgb(242, 246, 247)'}" title-color="black" :title="assetsType.type"></u-navbar>
		<view class="deposit m-30">
			<view class="d-flex align-items-center">
				<text class="d-block font-size-28 mr-10">{{i18n.convert2}}</text>
				<text class="iconfont icon-yanjing_xianshi text-white font-size-36" style="color: #fff !important;"></text>
			</view>
			<view class="mt-30">
				<view class="d-flex align-items-baseline">
					<text class="font-size-40 mr-20">{{balance.usdt_price}}</text>
					<text class="font-size-22">USDT</text>
				</view>
			</view>
			<view class="d-block font-size-22 opacity-50 mt-8"> ≈ {{(balance.usdt_price * $store.state.fiat.rate).toFixed(4)}} {{$store.state.fiat.currency_code}} </view>
			<view class="earnings-wrap">
				<view class="earnings font-size-22 d-grid-columns-2">
					<view class="">
						<text class="d-block font-size-22 opacity-50">{{i18n.availableQuota}}(USDT)</text>
						<text class="d-block font-size-22 mt-10">{{parseFloat(balance.balance)}}</text>
					</view>
					<view class="">
						<text class="d-block font-size-22 opacity-50">{{i18n.locked}}(USDT)</text>
						<text class="d-block font-size-22 mt-10">{{parseFloat(balance.lock_balance)}}</text>
					</view>
				</view>
			</view>
		</view>
		<view class="m-30">
			<u-subsection mode="button" button-background="linear-gradient(180deg, #00c6fb 0, #005bea)!important" :list="nav" active-color="#fff!important" radius="30" :current="0"></u-subsection>
			<view class="mt-30">
				<block v-if="list.length">
					<view class="p-30 box-shadow border-radius-20 mb-30 bg-white text-white" v-for="item in list">
						<view class="d-flex-between-center py-10">
							<text class="opacity-50">{{i18n.number}}</text>
							<text class="font-weight-bold"
								:style="{color:$utils.getColor(item.value)}">{{Number(item.value) + " " +item.currency_name}}</text>
						</view>
						<view class="d-flex-between-center py-10">
							<text class="opacity-50">{{i18n.time}}</text>
							<text class="font-weight-bold">{{item.created_time}}</text>
						</view>
						<view class="py-10 font-size-30">
							{{item.info}}
						</view>
					</view>

				</block>
				<default-page v-else></default-page>
			</view>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				nav: [{
					name: this.$t("fund.financialRecords")
				}],
				currency: 0,
				type_id: 0,
				type_name: '',
				balance: {},
				page: 1,
				list: [],
				canGet: true,
			};
		},
		onLoad(options) {
			const {
				currency,
				type_id,
				type_name
			} = options
			if (!currency || !type_id || !type_name) {
				this.$utils.showToast(this.$t("common.paramsWrong"))
				setTimeout(() => {
					uni.navigateBack({
						delta: 1
					})
				}, 1200)
				return
			}
			this.currency = currency
			this.type_id = type_id
			this.type_name = type_name
		},
		onShow() {
			
			// 获取特定币种在币币合约法币秒合约中的余额
			this.getDetail()
			// 获取特定币种特定方式的帐变记录
			this.getLegalLog()
		},
		methods: {
			getDetail() {
				const {
					currency,
					type_name: type
				} = this
				this.$u.api.wallet.getDetail(currency, type).then(res => {
					let balance = res.message
					if (this.type_id == 1) {
						balance.balance = Number(res.message.legal_balance);
						balance.lock_balance = Number(res.message.lock_legal_balance);
					} else if (this.type_id == 2) {
						balance.balance = Number(res.message.change_balance);
						balance.lock_balance = Number(res.message.lock_change_balance);
					} else if (this.type_id == 3) {
						balance.balance = Number(res.message.lever_balance);
						balance.lock_balance = Number(res.message.lock_lever_balance);
					} else if (this.type_id == 4) {
						balance.balance = Number(res.message.micro_balance);
						balance.lock_balance = Number(res.message.lock_micro_balance);
					}
					this.balance = balance

				})
			},
			getLegalLog() {
				if (!this.canGet) return
				const {
					currency,
					type_id: type,
					page
				} = this
				this.$u.api.wallet.getLegalLog(currency, type, page).then(res => {
					const list = res.message.list
					if (list.length) {
						this.list = [...this.list, ...list]
						this.page++
					} else {
						this.canGet = false
					}
				})
			}
		},
		computed: {
			i18n() {
				return this.$t("fund")
			},
			assetsType(){
				const i18n = this.$t("fund")
				const assetsTypes = [{
					id: 1,
					name: 'legal',
					type: i18n.fiat + i18n.account,
				}, {
					id: 2,
					name: 'change',
					type: i18n.exchange + i18n.account,
				}, {
					id: 3,
					name: 'lever',
					type: i18n.leverage + i18n.account,
				}, {
					id: 4,
					name: 'micro',
					type: i18n.second + i18n.account,
				}]
				
				return assetsTypes.find(item => item.id == this.type_id)
			}
		},
		onReachBottom() {
			this.getLegalLog()
		}
	}
</script>

<style lang="scss" scoped>
.linear-gradient-button {
	background: linear-gradient(180deg,#00c6fb 0,#005bea);
}
.deposit {
	background: linear-gradient(180deg,#00c6fb 0,#005bea);
}
.u-subsection .u-item-bg {
	background: linear-gradient(180deg,#00c6fb 0,#005bea) !important;
	color: #fff !important;
	border-radius: 34rpx !important;
}
</style>
