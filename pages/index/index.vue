<template>
	<view class="container">
		<view class="title">
			<text>水果设计</text>
		</view>
		<view class="options">
			<view class="option-item" @click="navigateTo('/pages/border/border')" style="background-color: #FFE4E1;">
				<!-- <image src="/static/frame.png" mode="aspectFit"></image> -->
				<text>选边框</text>
			</view>
			<view class="option-item" @click="navigateTo('/pages/fruit/fruit')" style="background-color: #E0FFFF;">
				<!-- <image src="/static/fruit.png" mode="aspectFit"></image> -->
				<text>选果图</text>
			</view>
			<view class="option-item" @click="navigateTo('/pages/poster/poster')" style="background-color: #F0FFF0;">
				<!-- <image src="/static/poster.png" mode="aspectFit"></image> -->
				<text>海报设计</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			navigateTo(url) {
				uni.navigateTo({
					url: url
				})
			}
		}
	}
</script>

<style lang="scss">
.container {
	padding: 20px;
	
	.title {
		text-align: center;
		font-size: 24px;
		font-weight: bold;
		margin-bottom: 40px;
	}
	
	.options {
		display: grid;
		grid-template-columns: repeat(2, 1fr);
		gap: 20px;
		padding: 0 10px;
		
		.option-item {
			aspect-ratio: 1;
			border-radius: 12px;
			padding: 20px;
			box-shadow: 0 2px 12px rgba(0, 0, 0, 0.1);
			display: flex;
			flex-direction: column;
			align-items: center;
			justify-content: center;
			gap: 15px;
			
			image {
				width: 80px;
				height: 80px;
			}
			
			text {
				font-size: 18px;
				color: #333;
				font-weight: bold;
			}
			
			&:active {
				transform: scale(0.98);
				opacity: 0.9;
			}
		}
	}
}
</style>
