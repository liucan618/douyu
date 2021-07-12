<template>
	<view v-if="data" style="padding: 16rpx;">
		<view style="border-bottom: 1px solid gray; padding-bottom: 4px;">
			{{ data.details.lname }}
		</view>

		<swiper
			:indicator-dots="true"
			:autoplay="true"
			:interval="3000"
			:duration="250"
			:circular="true"
			style="height: 718rpx;"
		>
			<swiper-item v-for="(item, index) in data.details.picList" :key="index">
				<image
					style="height: 100%;"
					:src="'http://101.96.128.94:9999/' + item.md"
					mode=""
				></image>
			</swiper-item>
		</swiper>

		<view style="font-size: 1.2em; font-weight: bold;">
			{{ data.details.title }}
		</view>
		<view style="margin: 5px 0;">{{ data.details.subtitle }}</view>
		<view style="font-weight: bold; color: red; font-size: 1.2em;">
			¥{{ data.details.price }}
		</view>

		<uni-collapse>
			<uni-collapse-item title="更多商品推荐">
				<uni-list>
					<uni-list-item
						v-for="(item, index) in data.family.laptopList"
						:key="index"
						:title="item.spec"
						:showArrow="true"
						:to="`/pages/xuezi-detail/xuezi-detail?lid=${item.lid}`"
						:clickable="true"
					></uni-list-item>
				</uni-list>
			</uni-collapse-item>
		</uni-collapse>
		
		<!-- {{}} 中的值做修改  用 过滤器 -->
		<!-- "" 中的值做修改: 用 计算属性 -->
		<view v-html="details">
			
		</view>
		
		<uni-goods-nav />
	</view>
</template>

<script>
export default {
	data() {
		return {
			lid: '',
			data: null
		};
	},
	onLoad(options) {
		this.lid = options.lid;
		this.getData();
	},
	methods: {
		getData() {
			uni.request({
				url: 'http://101.96.128.94:9999/data/product/details.php',
				method: 'GET',
				data: { lid: this.lid },
				success: res => {
					console.log(res);
					this.data = res.data;
				},
				fail: () => {},
				complete: () => {}
			});
		}
	},
	computed: {
		details() {
			let details = this.data.details.details;
			// 返回值的详情内容是 网页内容, 但是其中的图片地址都是相对路径, 必须改成绝对路径才可用!
			// src="img   改成  src="http://101.96.128.94:9999/img
			details = details.replace(/src="img/g, 'width="100%" src="http://101.96.128.94:9999/img');
			
			// src="//img20   改成  src="https://img20
			details = details.replace(/src="\/\/img20/g, 'width="100%" src="https://img20');
			
			return details;
		}
	},
};
</script>

<style lang="scss"></style>
