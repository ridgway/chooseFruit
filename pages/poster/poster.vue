<template>
	<view class="poster-list">
		<view v-for="(item, idx) in posters" :key="idx" class="poster-item">
			<image :src="item.img" class="poster-img" mode="widthFix" />
			<view class="poster-title">{{ item.title }}</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				posters: [
					{ img: '/static/poster/1.jpg', title: '海报1' },
					{ img: '/static/poster/2.jpg', title: '海报2' },
					{ img: '/static/poster/3.jpg', title: '海报3' },
					{ img: '/static/poster/4.jpg', title: '海报4' },
					{ img: '/static/poster/5.jpg', title: '海报5' }
				]
			}
		}
	}
</script>

<style lang="scss">
.poster-list {
	padding: 24rpx;
	.poster-item {
		margin-bottom: 40rpx;
		background: #fff;
		border-radius: 16rpx;
		box-shadow: 0 2rpx 8rpx rgba(0,0,0,0.04);
		padding: 24rpx;
		display: flex;
		flex-direction: column;
		align-items: center;
		.poster-img {
			width: 100%;
			max-width: 600rpx;
			border-radius: 12rpx;
			margin-bottom: 16rpx;
			background: #f2f2f2;
		}
		.poster-title {
			font-size: 28rpx;
			color: #333;
			text-align: center;
			font-weight: bold;
		}
	}
}
</style> 