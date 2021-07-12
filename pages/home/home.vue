<template>
	<view v-if="data">
		<uni-notice-bar
			:text="notice"
			:single="true"
			:showIcon="true"
			:scrollable="true"
			:speed="70"
			style="margin-bottom: 0;"
		></uni-notice-bar>

		<uni-swiper-dot
			:info="data.carouselItems"
			:current="current"
			field="title"
			mode="nav"
		>
			<swiper
				:autoplay="true"
				:interval="3000"
				:duration="250"
				@change="swiperChanged"
				:circular="true"
			>
				<swiper-item v-for="(item, index) in data.carouselItems">
					<image
						style="width: 100%; height: 100%;"
						:src="'http://101.96.128.94:9999/' + item.img"
						mode=""
					></image>
				</swiper-item>
			</swiper>
		</uni-swiper-dot>

		<view v-for="(item, index) in content" :key="index">
			<view style="padding: 10px; background-color: #DCDFE6;">
				<uni-icons :type="item.icon"></uni-icons>
				<text>{{ item.title }}</text>
			</view>

			<uni-grid
				:column="2"
				:showBorder="false"
				:square="false"
				:highlight="false"
				style="margin-bottom: 10px;"
			>
				<uni-grid-item v-for="(v, k) in item.data" :key="k">
					<view
						class="item"
						:style="{ marginLeft: k % 2 == 0 ? '15rpx' : '7rpx' }"
					>
						<image
							:src="'http://101.96.128.94:9999/' + v.pic"
							mode=""
							style="width: 100%; height: 300rpx; "
						></image>
						<view style="padding: 4px;">
							<view style="font-weight: bold;" class="single-line">
								{{ v.title }}
							</view>
							<view style="font-size: 0.8em;" class="single-line">
								{{ v.details }}
							</view>
							<!-- <view class="btn">查看详情</view> -->
							<button type="primary" size="mini" style="margin-top: 6px;">
								查看详情
							</button>
						</view>
					</view>
				</uni-grid-item>
			</uni-grid>
		</view>

		<uni-grid :column="4" :showBorder="false">
			<uni-grid-item v-for="(item, index) in icons" :key="index">
				<view style="display: flex; flex-direction: column; align-items: center;">
					<image
						:src="`/static/icons/${item.icon}`"
						mode=""
						style="width: 100rpx; height: 100rpx;"
					></image>
					<text style="color: #808080;">{{ item.title }}</text>
				</view>
			</uni-grid-item>
		</uni-grid>
	</view>
</template>

<script>
export default {
	data() {
		return {
			icons: [
				{ icon: 'icon1.png', title: '品质保障' },
				{ icon: 'icon2.png', title: '私人订制' },
				{ icon: 'icon3.png', title: '学员特供' },
				{ icon: 'icon4.png', title: '专属特权' }
			],
			current: 0,
			data: null,
			// #ifdef APP-PLUS
			notice: '欢迎web2011同学使用 学子商城App, 期待同学能够选购到满意的商品',
			// #endif
			// #ifdef H5
			notice: '欢迎web2011同学使用 学子商城网站, 期待同学能够选购到满意的商品',
			// #endif
			// #ifdef MP
			notice: '欢迎web2011同学使用 学子商城小程序, 期待同学能够选购到满意的商品'
			// #endif
		};
	},
	onLoad() {
		this.getData();
	},
	methods: {
		swiperChanged(e) {
			// console.log(e);
			this.current = e.detail.current;
		},
		getData() {
			uni.request({
				url: 'http://101.96.128.94:9999/data/product/index.php',
				method: 'GET',
				data: {},
				success: res => {
					console.log(res);
					this.data = res.data;
				},
				fail: () => {},
				complete: () => {}
			});
		}
	},
	// 计算属性: 利用已有数据项, 经过操作得到新的数据项
	computed: {
		content() {
			return [
				{
					icon: 'shop',
					title: '1F / 首页推荐',
					data: this.data.recommendedItems
				},
				{
					icon: 'shop',
					title: '2F / 最新上架',
					data: this.data.newArrivalItems
				},
				{ icon: 'shop', title: '3F / 热销单品', data: this.data.topSaleItems }
			];
		}
	}
};
</script>

<style>
.item {
	border: 1px solid gray;
	border-radius: 4px;
	box-shadow: #333333 0px 0px 2px;
	width: 353rpx;
	/* (750 - 350 *2)/3 =  */
	margin-top: 10px;
}

.single-line {
	white-space: nowrap;
	overflow: hidden;
	text-overflow: ellipsis;
}

.btn {
	background-color: #4cd964;
	display: inline-block;
	padding: 3px 6px;
	border-radius: 4px;
}
</style>
