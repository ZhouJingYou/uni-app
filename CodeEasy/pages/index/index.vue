<template>
	<view class="home-page">
		<button @click="chooseImage" class='flex-center'>上传图片</button>
		<view class="imgList">
			<view class="img-wrap" v-for="(item,index) in imgList" :key="index">
				<image :src="item" mode="widthFix" @click="previewImg(item)" @longtap="showMenu(index)">
				</image>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				imgList: []
			}
		},
		onLoad(options) {
			console.log('onLoad');
			let res = uni.getStorageInfoSync()
			console.log("getStorageInfoSync res: ", res);
			let storage = uni.getStorageSync('imgList')
			if (storage) this.imgList = JSON.parse(uni.getStorageSync('imgList'));
			if (this.imgList.length) this.previewImg(this.imgList[0])
		},
		methods: {
			async chooseImage() {
				uni.chooseImage({
					count: 1,
					success: res => {
						let tempFilePaths = res.tempFilePaths;
						uni.saveFile({
							tempFilePath: tempFilePaths[0],
							success: res2 => {
								console.log(this, 'this')
								this.imgList.push(res2.savedFilePath)
								console.log("this.imgList: ", this.imgList);
								uni.setStorageSync('imgList', JSON.stringify(this.imgList))
							}
						});
					}
				});
			},
			previewImg(current) {
				console.log(current, 'current');
				console.log(this.imgList, 'this.imgList');
				uni.previewImage({
					urls: this.imgList,
					current,
					loop: true,

				})
			},
			showMenu(index) {
				uni.showActionSheet({
					itemList: ['置顶', '上移', '下移', '删除'],
					success: (res) => {
						let map = {
							0: 'top',
							1: 'up',
							2: 'down',
							3: 'delete',
						}
						this[map[res.tapIndex]](index)
					},
					fail: (res) => {
						console.log(res.errMsg);
					}
				});
			},
			top(index) {
				if (!index) return
				let imgArr = this.imgList.splice(index, 1)
				this.imgList.unshift(...imgArr)
				uni.setStorageSync('imgList', JSON.stringify(this.imgList))
			},
			up(index) {
				if (!index) return
				let imgTemp = this.imgList[index]
				this.imgList.splice(index, 1, this.imgList[index - 1])
				this.imgList.splice(index - 1, 1, imgTemp)
				uni.setStorageSync('imgList', JSON.stringify(this.imgList))
			},
			down(index) {
				if (index === this.imgList.length - 1) return
				let imgTemp = this.imgList[index]
				this.imgList.splice(index, 1, this.imgList[index + 1])
				this.imgList.splice(index + 1, 1, imgTemp)
				uni.setStorageSync('imgList', JSON.stringify(this.imgList))
			},
			delete(index) {
				uni.showModal({
					title: '提示',
					content: '确定删除图片吗?',
					success: (res) => {
						if (res.confirm) {
							this.imgList.splice(index, 1)
							uni.setStorageSync('imgList', JSON.stringify(this.imgList))
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				});
			}
		}

	}
</script>

<style lang="scss">
	.home-page {
		display: flex;
		align-items: center;
		flex-direction: column;
		justify-content: flex-start;
	}

	.flex-center {
		display: flex;
		align-items: center;
		justify-content: center;
	}

	/* .imgList {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	} */

	.img-wrap {
		width: 100vw;

		image {
			width: 100%;
		}
	}

	button {
		width: 100%;
	}
</style>
