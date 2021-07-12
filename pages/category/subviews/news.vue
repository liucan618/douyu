<template>
	<scroll-view
		:style="{ height: wh + 'px' }"
		:scroll-y="true"
		@scrolltolower="loadMore"
		:refresher-enabled="true"
		@refresherrefresh="onRefresh"
		:refresher-triggered="refreshing"
	>
		<uni-list>
			<uni-list-item
				:clickable="true"
				v-for="(item, index) in result"
				:key="index"
				:to="`/pages/news-detail/news-detail?path=${item.path}`"
			>
				<image
					:src="item.image"
					style="width: 250rpx; height: 150rpx; min-width: 250rpx; margin-right: 10px;"
					slot="header"
					mode=""
				></image>

				<view
					slot="body"
					style="display: flex; flex-direction: column; justify-content: space-around;"
				>
					<text>{{ item.title }}</text>
					<text style="color:gray">{{ item.passtime }}</text>
				</view>
			</uni-list-item>
		</uni-list>

		<uni-load-more :status="more"></uni-load-more>
	</scroll-view>
</template>

<script>
export default {
	props: ['wh'],
	data() {
		return {
			result: [],
			page: 1, //接口不提供页数返回值, 只能自己写
			more: 'more',
			refreshing: false
		};
	},
	// 千万不要写 onLoad();  因为这是 页面的生命周期
	// 当前文件是 组成 分类页面的 局部,  属于组件;  不需要在 pages.json 中注册
	mounted() {
		this.getData();
	},
	methods: {
		onRefresh(e) {
			if (this.refreshing) return;

			this.refreshing = true;

			uni.request({
				url: 'https://api.apiopen.top/getWangYiNews',
				method: 'GET',
				data: { page: 1 },
				success: res => {
					this.result = res.data.result;
					this.page = 1;
					this.refreshing = false;
				},
				fail: () => {},
				complete: () => {}
			});
		},
		loadMore() {
			this.more = 'loading';

			uni.request({
				url: 'https://api.apiopen.top/getWangYiNews',
				method: 'GET',
				data: { page: this.page + 1 },
				success: res => {
					console.log(res);

					// 返回值 只有新的数组 没有其他值, 直接合并到本地即可
					this.result = this.result.concat(res.data.result);

					this.page++; //接口没有页数, 只能自己更新

					this.more = 'more';
				},
				fail: () => {},
				complete: () => {}
			});
		},
		getData() {
			uni.request({
				url: 'https://api.apiopen.top/getWangYiNews',
				method: 'GET',
				data: { page: 1 },
				success: res => {
					console.log(res);
					this.result = res.data.result;
				},
				fail: () => {},
				complete: () => {}
			});
		}
	}
};
</script>

<style></style>
