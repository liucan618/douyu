<template>
	<web-view :src="path"></web-view>
</template>

<script>
export default {
	data() {
		return {
			path:''
		};
	},
	onLoad(options) {
		this.path = options.path;
	}
};
</script>

<style lang="scss"></style>
