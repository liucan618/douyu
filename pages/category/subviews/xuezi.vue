<template>
	<!-- 
	 refresher-enabled: 开启 自定义下拉刷新
	 @refresherrefresh: 自定义下拉刷新被触发
	 refresher-triggered: 刷新动画的显示状态
	 -->
	<scroll-view
		@scrolltolower="loadMore"
		v-if="data"
		:scroll-y="true"
		:style="{ height: wh+'px' }"
		:refresher-enabled="true"
		@refresherrefresh="onRefresh"
		:refresher-triggered="refreshing"
	>
		<uni-list>
			<uni-list-item :clickable="true" :to="`/pages/xuezi-detail/xuezi-detail?lid=${item.lid}`" v-for="(item, index) in data.data" :key="index">
				<image
					style="width: 200rpx; height: 200rpx; min-width: 200rpx; margin-right: 10px;"
					slot="header"
					:src="'http://101.96.128.94:9999/' + item.pic"
					mode=""
				></image>
				<view
					slot="body"
					style="display: flex; flex-direction: column; justify-content: space-around;"
				>
					<text class="title">{{ item.title }}</text>
					<text class="price">¥{{ item.price }}</text>
				</view>
			</uni-list-item>
		</uni-list>

		<uni-load-more :status="more"></uni-load-more>
	</scroll-view>
</template>

<script>
export default {
	// 组件声明属性, 代表当前组件 有一个 wh 属性, 使用时  <组件名 wh="值" />
	props: ['wh'],
	data() {
		return {
			more: 'more',
			data: null,
			refreshing: false, //代表当前下拉刷新动画的状态
		};
	},
	mounted() {
		this.getData();
	},
	methods: {
		onRefresh(){
			// 当前已经是下拉刷新状态, 不要再次触发
			if (this.refreshing) return;
			
			console.log('下拉刷新');
			this.refreshing = true;
			
			uni.request({
				url: 'http://101.96.128.94:9999/data/product/list.php',
				method: 'GET',
				data: {pno:1},
				success: res => {
					console.log(res);
					this.data = res.data;
					
					this.refreshing = false;
				},
				fail: () => {},
				complete: () => {}
			});
		},
		loadMore() {
			// 加载更多之前 要判断是否还有更多数据, 如果没有, 则不再加载更多
			const {pno, pageCount} = this.data;
			if (pno == pageCount) return; //return 终止代码执行
			
			this.more = 'loading';
			
			uni.request({
				url: 'http://101.96.128.94:9999/data/product/list.php',
				method: 'GET',
				data: {pno: this.data.pno + 1},
				success: res => {
					console.log(res);
					// 返回值中包含 新的数据 和 新的页数
					// 需要把数据进行合并,  页数进行更新
					
					// 旧数据 和 新数据合并,  其中 新数据应该是主体, 这样页数才能更新
					res.data.data = this.data.data.concat(res.data.data)
					this.data = res.data; // 旧数据 = 新数据,  页数才能更新
					
					// 错误写法:  只写  this.data.data = this.data.data.concat(res.data.data)
					// 不使用上方写法, 则 页数pno 没有更新, 则加载更多 永远都是 加载 第二页的数据
					
					const {pno, pageCount} = this.data; // 当前页 和 总页数
					this.more = pno == pageCount ? 'noMore':'more';
					//noMore 代表没有更多数据
				},
				fail: () => {},
				complete: () => {}
			});
		},
		getData() {
			uni.request({
				url: 'http://101.96.128.94:9999/data/product/list.php',
				method: 'GET',
				data: { pno: 1 },
				success: res => {
					console.log(res);
					this.data = res.data;
				},
				fail: () => {},
				complete: () => {}
			});
		}
	}
};
</script>

<style>
.price {
	color: red;
	font-size: 1.2em;
}

.title {
	text-overflow: -o-ellipsis-lastline;
	overflow: hidden;
	text-overflow: ellipsis;
	display: -webkit-box;
	-webkit-line-clamp: 2;
	line-clamp: 2;
	-webkit-box-orient: vertical;
}
</style>
