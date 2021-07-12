<template>
	<view>
		<scroll-view
			:scroll-into-view="'ss' + current"
			:scroll-x="true"
			style="background-color: #007AFF; padding-top: 20px; position: fixed; top: 0; left: 0; right: 0; z-index: 2;"
		>
			<view class="tab">
				<!-- id 配合 scroll-into-view使用 -->
				<text
					:id="'ss' + index"
					@click="chooseTab(index)"
					:class="{ cur: current == index }"
					v-for="(item, index) in tabs"
					:key="index"
				>
					{{ item }}
				</text>
			</view>
		</scroll-view>

		<swiper
			:current="current"
			@change="changeSw"
			:style="{ height: wh + 'px' }"
			style="margin-top: 64px;"
		>
			<swiper-item v-for="(item, index) in tabs" :key="index">
				<!-- 当序号 被访问时, 再挂载对应页面:  visited 中存储了已访问的页面序号 -->
				<!-- indexOf(): 查找一个元素在 数组中的位置, 返回值 -1 代表没有-->

				<!-- 组件的高度 不在组件中读取, 而是父传入 -->
				<xuezi :wh="wh" v-if="index == 0 && visited.indexOf(0) != -1"></xuezi>
				<news :wh="wh" v-if="index == 1 && visited.indexOf(1) != -1"></news>
				<meitu :wh="wh" v-if="index == 2 && visited.indexOf(2) != -1"></meitu>
				<daiding v-if="index > 2"></daiding>
			</swiper-item>
		</swiper>
	</view>
</template>

<script>
// 引入子组件
import xuezi from './subviews/xuezi.vue';
import meitu from './subviews/meitu.vue';
import daiding from './subviews/daiding.vue';
import news from './subviews/news.vue';

export default {
	data() {
		return {
			//保存已访问的页面序号:
			visited: [0], //默认 序号0 页面被访问
			tabs: [
				'学子商城',
				'网易新闻',
				'穿搭推荐',
				'待定',
				'待定',
				'待定',
				'待定',
				'待定',
				'待定'
			],
			current: 0, //当前项
			wh: 0
		};
	},
	mounted() {
		this.wh = uni.getSystemInfoSync().windowHeight;
	},
	// 注册组件
	components: { xuezi, meitu, daiding, news },
	methods: {
		changeSw(e) {
			// console.log(e);
			this.current = e.detail.current;
			// 滑动到某个页面时, 把此页面的序号 添加为已访问

			this.visited.push(this.current);
		},
		chooseTab(index) {
			this.current = index;
		}
	}
};
</script>

<style lang="scss">
.tab {
	display: flex;

	text {
		white-space: nowrap;
		padding: 10px;
		color: white;
	}

	.cur {
		color: orange;
		border-bottom: 3px solid orange;
		font-weight: bold;
	}
}
</style>
