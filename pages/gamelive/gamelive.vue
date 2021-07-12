<template>
	<view>
		<scroll-view :scroll-x="true">
			<view style="display: flex;">
				<text
					style="white-space: nowrap; padding: 10px;"
					v-for="(item, index) in cates"
					:key="index"
					:class="{ cur: current == index }"
					@click="changeSw(index)"
				>
					{{ item.name }}
				</text>
			</view>
		</scroll-view>

		<scroll-view
			:scroll-y="true"
			:style="{ height: wh - 136 + 'px' }"
			v-if="lives && showlist"
			@scrolltolower="loadMore"
			:refresher-enabled="true"
			:refresher-triggered="refreshing"
			@refresherrefresh="onRefresh"
		>
			<uni-grid :column="2" :showBorder="false" :square="false" @change="click">
				<uni-grid-item :index="index" v-for="(item, index) in lives.list" :key="index">
					<view class="item">
						<view style="position: relative; ">
							<image
								style="width: 100%; height: 220rpx;line-height: 0; "
								:src="item.roomSrc"
								mode=""
							></image>

							<view
								style="position: absolute; right: 0; top: 0; background-color: rgba(0,0,0,0.3); color:white; font-size: 0.8em;"
							>
								{{ item.hn }}
							</view>

							<view
								style="display: flex; align-items: center; position: absolute; left: 0; bottom: 10rpx; background-color: rgba(0,0,0,0.3);"
							>
								<image
									:src="item.avatar"
									style="width: 40rpx; height: 40rpx;"
									mode=""
								></image>
								<text style="color: white;">{{ item.nickname }}</text>
							</view>
						</view>

						<view>{{ item.roomName }}</view>
					</view>
				</uni-grid-item>
			</uni-grid>
			
			<uni-load-more :status="more"></uni-load-more>
		</scroll-view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			cates: [],
			current: 0,
			wh: uni.getSystemInfoSync().windowHeight,
			lives: null,
			more: 'more',
			showlist: true,
			refreshing: false,
		};
	},
	mounted() {
		this.getCates();
	},
	methods: {
		click(e){
			console.log(e);
			
			const index = e.detail.index;
			
			const rid = this.lives.list[index].rid;
			
			uni.navigateTo({
				url:'/pages/live-detail/live-detail?rid='+rid
			})
		},
		onRefresh(){
			this.refreshing = true;
			const shortName = this.cates[this.current].shortName;
			
			uni.request({
				url: 'https://m.douyu.com/api/room/list',
				method: 'GET',
				data: {page:1, type: shortName},
				success: res => {
					this.lives = res.data.data;
					
					this.refreshing = false;
				},
				fail: () => {},
				complete: () => {}
			});
		},
		loadMore(){
			this.more = 'loading';
			const shortName = this.cates[this.current].shortName;
			
			uni.request({
				url: 'https://m.douyu.com/api/room/list',
				method: 'GET',
				data: {page: this.lives.nowPage+1, type: shortName},
				success: res => {
					console.log(res);
					
					res.data.data.list  = this.lives.list.concat(res.data.data.list);
					this.lives = res.data.data;
					
					this.more = 'more';
				},
				fail: () => {},
				complete: () => {}
			});
		},
		changeSw(index){
			if (this.current == index) return;
			
			this.showlist = false;
			
			this.$nextTick(()=>{
				this.showlist = true
			})
			
			this.current = index
			this.getLives(index)
		},
		getLives(index) {
			const shortName = this.cates[index].shortName;

			uni.request({
				url: 'https://m.douyu.com/api/room/list',
				method: 'GET',
				data: { page: 1, type: shortName },
				success: res => {
					console.log(res);

					this.lives = res.data.data;
				},
				fail: () => {},
				complete: () => {}
			});
		},
		getCates() {
			uni.request({
				url: 'https://m.douyu.com/api/cate/recList',
				method: 'GET',
				data: {},
				success: res => {
					console.log(res);
					this.cates = res.data.data;

					this.getLives(0);
				},
				fail: () => {},
				complete: () => {}
			});
		}
	}
};
</script>

<style>
.cur {
	color: orange;
	border-bottom: 2px solid orange;
}

.item {
	width: 350rpx; 
	margin-left: 15rpx;
	margin-bottom: 10px;
}
</style>
