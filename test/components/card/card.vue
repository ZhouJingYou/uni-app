<template>
	<view>
		<text @click="handleClick">card</text>
	</view>
</template>

<script>
	export default {
		name: "card",
		data() {
			return {

			};
		},
		methods: {
			handleClick() {
				this.$emit('click')

			}
		}
	}
</script>

<style>

</style>
