<template>
	<view style="display: flex;">
		<scroll-view
			style="width: 250rpx;"
			:scroll-y="true"
			:style="{ height: wh + 'px' }"
		>
			<view
				:class="{ cur: index == current }"
				v-for="(item, index) in heros"
				:key="index"
				class="left-cell"
				@click="chooseHero(index)"
			>
				<text>{{ item.title }}</text>
			</view>
		</scroll-view>
		
		<scroll-view
			style="width: 500rpx; background-color: lightgray;"
			:scroll-y="true"
			:style="{ height: wh + 'px' }"
			v-if="data && showDetail"
			
		>
			<view style="padding: 10px;">
				<view style="display: flex;">
					<image
						:src="
							`https://game.gtimg.cn/images/lol/act/img/champion/${
								data.hero.alias
							}.png`
						"
						mode=""
						style="width: 150rpx; height: 150rpx;"
					></image>
					<view style="margin-left: 10px;">
						<view>{{data.hero.title}}</view>
						<view style="font-size: 0.8em;">昵称: {{data.hero.name}}</view>
						<view style="font-size: 0.8em;">金币: {{data.hero.goldPrice}}</view>
						<view style="font-size: 0.8em;">点券: {{data.hero.couponPrice}}</view>
					</view>
				</view>
				
				<uni-card title="背景故事" :isFull="true">
					<text style="font-size: 0.7em;">{{data.hero.shortBio}}</text> 
				</uni-card>
				
				<uni-collapse>
					<uni-collapse-item title="使用建议">
						<view style="background-color: white; margin: 4px;" v-for="(item,index) in data.hero.allytips" :key="index">
							<text style="font-size: 0.7em;" >{{item}}</text> 
						</view>
					</uni-collapse-item>
					
					<uni-collapse-item title="对战技巧">
						<view style="background-color: white; margin: 4px;" v-for="(item,index) in data.hero.enemytips" :key="index">
							<text style="font-size: 0.7em;" >{{item}}</text> 
						</view>
					</uni-collapse-item>
					
					<uni-collapse-item title="皮肤">
						<uni-grid :column="3">
							<uni-grid-item v-for="(item,index) in data.skins">
								<image  :src="item.iconImg" mode="aspectFill" style="width: 100%; height: 100%; border-radius: 4px;"></image>
							</uni-grid-item>
						</uni-grid>
					</uni-collapse-item>
					
					<uni-collapse-item title="技能">
						<uni-grid :column="3">
							<uni-grid-item v-for="(item,index) in data.spells" :key="index">
								<image :src="item.abilityIconPath" mode="aspectFill" style="width: 100%; width: 100%;"></image>
							</uni-grid-item>
						</uni-grid>
					</uni-collapse-item>
				</uni-collapse>
				
			
			</view>
		</scroll-view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			heros: [],
			wh: uni.getSystemInfoSync().windowHeight,
			current: 0,
			data: null,
			showDetail: true,
		};
	},
	mounted() {
		this.getHeros();
	},
	methods: {
		chooseHero(index) {
			// 防止已显示项目 重复点击:  判断如果点击的是当前项, 不做任何事情
			if (this.current== index) return;
			
			this.current = index;

			this.getDetail(this.heros[index].heroId);
			
			// 刷新页面: 先隐藏 再显示
			this.showDetail = false; 
			
			// 先隐藏, 延时执行显示
			this.$nextTick(()=>{
				this.showDetail = true;
			})
		},
		getHeros() {
			uni.request({
				url:
					'https://game.gtimg.cn/images/lol/act/img/js/heroList/hero_list.js?v=50',
				method: 'GET',
				data: {},
				success: res => {
					console.log(res);
					this.heros = res.data.hero;

					this.getDetail(0);
				},
				fail: () => {},
				complete: () => {}
			});
		},
		getDetail(hid) {
			uni.request({
				url: `https://game.gtimg.cn/images/lol/act/img/js/hero/${hid}.js`,
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
	}
};
</script>

<style lang="scss">
.left-cell {
	background-color: white;
	border-bottom: 1px solid gray;
	padding: 8px;

	text {
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
	}
}

.cur {
	background-color: lightgray;
	color: #007aff;
}
</style>
