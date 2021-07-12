<template>
	<web-view :src="`https://m.douyu.com/${this.rid}`"></web-view>
</template>

<script>
export default {
	data() {
		return {
			rid:''
		};
	},
	onLoad(options) {
		this.rid = options.rid
	}
};
</script>

<style lang="scss"></style>
