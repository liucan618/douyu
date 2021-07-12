<template>
	<scroll-view
		@scrolltolower="loadMore"
		:scroll-y="true"
		:style="{ height: wh - 64 + 'px' }"
		:refresher-enabled="true"
		@refresherrefresh="onRefresh"
		:refresher-triggered="refreshing"
	>
		<uni-grid :column="3" :showBorder="false" :square="false" @change="chooseItem">
			<uni-grid-item :index="index" v-for="(item, index) in result" :key="index">
				<image
					:src="item.img"
					style="width: 95%; height: 300rpx; margin-left: 10rpx; margin-top: 20rpx; border-radius: 3px; box-shadow: #666666 0 0 4px; "
					mode="aspectFill"
					
				></image>
			</uni-grid-item>
		</uni-grid>

		<uni-load-more :status="more"></uni-load-more>
	</scroll-view>
</template>

<script>
export default {
	props: ['wh'],
	data() {
		return {
			result: [],
			more: 'more',
			page: 7,
			refreshing: false,
		};
	},
	mounted() {
		this.getData();
	},
	methods: {
		chooseItem(e){
			console.log(e);
			const index = e.detail.index;
			const img = this.result[index].img;
			
			uni.previewImage({
				urls:[img]
			})
			
		},
		onRefresh(){
			if (this.refreshing) return;
			
			this.refreshing = true;
			
			uni.request({
				url: 'https://api.apiopen.top/getImages',
				method: 'GET',
				data: {page: 7},
				success: res => {
					this.result = res.data.result;
					this.page = 7
					
					this.refreshing = false;
				},
				fail: () => {},
				complete: () => {}
			});
		},
		loadMore() {
			this.more = 'more';

			uni.request({
				url: 'https://api.apiopen.top/getImages',
				method: 'GET',
				data: { page: this.page + 1 },
				success: res => {
					console.log(res);

					this.result = this.result.concat(res.data.result);
					this.page++;
				},
				fail: () => {},
				complete: () => {}
			});
		},
		getData() {
			uni.request({
				url: 'https://api.apiopen.top/getImages',
				method: 'GET',
				data: { page: 7 },
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
