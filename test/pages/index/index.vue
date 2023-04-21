<template>
	<view class="content">
		<image class="logo" src="/static/logo.png"></image>
		<view class="text-area">
			<text class="title" @>{{title}}</text>
		</view>
		<card @click='onclick'></card>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Hello'
			}
		},
		onLoad() {
			console.log('page onload');
		},
		onReady() {
			console.log('page onready');
		},
		onShow() {
			console.log('page onshow');
		},
		onUnload() {
			console.log('page onunload');
		},
		onHide() {
			console.log('page onHide');
		},
		methods: {
			onclick() {
				uni.showLoading({
					title: '正在加载中~',
					mask: false
				});

			}

		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;

	}
</style>
