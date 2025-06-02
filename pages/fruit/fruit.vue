<template>
	<view class="container">
		<view class="header">
			<view class="btn-group">
				<button class="action-btn copy-btn" @tap="copyToClipboard">
					一键复制所有选中的水果
				</button>
				<!-- <button class="action-btn export-btn" @tap="exportToExcel">导出Excel</button> -->
			</view>
			<view class="action-text">点击"复制"按钮后，您所有选中的水果都已复制，请直接粘贴给小伙伴</view>
		</view>
		<view class="tab-bar">
			<view v-for="(tab, idx) in tabs" :key="idx" class="tab-item" :class="{ active: activeTab === idx }" @click="activeTab = idx">
				{{ tab.name }}
			</view>
		</view>
		<view class="content">
			<!-- 左侧水果列表 -->
			<view class="fruit-list">
				<view 
					class="fruit-item" 
					v-for="(item, index) in currentFruits" 
					:key="index"
					:class="{'selected': isSelected(item.name)}"
					@tap="selectFruit(item)"
				>
					<text>{{item.name}}</text>
				</view>
			</view>
			
			<!-- 右侧已选水果 -->
			<view class="selected-list">
				<view class="selected-title">
					<text>已选水果</text>
					<text class="clear-all" @tap="clearAll">清空</text>
				</view>
				<view class="selected-items">
					<template v-for="(category, categoryName) in selectedFruitsByCategory">
						<view v-if="category.length > 0" :key="categoryName" class="category-group">
							<view class="category-title">{{categoryName}}：</view>
							<view class="category-items">
								<view 
									class="selected-item" 
									v-for="(item, index) in category" 
									:key="index"
								>
									<text>{{item}}</text>
									<text class="delete-btn" @tap="removeFruit(categoryName, item)">×</text>
								</view>
							</view>
						</view>
					</template>
					<view class="empty-tip" v-if="!hasSelectedFruits">
						<text>请从左侧选择水果</text>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
import { fruitData } from '@/fdata/nameData'
export default {
	data() {
		return {
			activeTab: 0,
			tabs: fruitData,
			selectedFruitsByCategory: {}
		}
	},
	computed: {
		currentFruits() {
			return this.tabs[this.activeTab].data
		},
		hasSelectedFruits() {
			return Object.values(this.selectedFruitsByCategory).some(category => category.length > 0)
		}
	},
	methods: {
		isSelected(name) {
			const currentCategory = this.tabs[this.activeTab].name
			return this.selectedFruitsByCategory[currentCategory]?.includes(name)
		},
		selectFruit(fruit) {
			const category = this.tabs[this.activeTab].name
			if (!this.selectedFruitsByCategory[category]) {
				this.$set(this.selectedFruitsByCategory, category, [])
			}
			
			if (!this.selectedFruitsByCategory[category].includes(fruit.name)) {
				this.selectedFruitsByCategory[category].push(fruit.name)
				uni.showToast({
					title: '已添加：' + fruit.name,
					icon: 'success'
				})
			}
		},
		removeFruit(category, name) {
			const index = this.selectedFruitsByCategory[category].indexOf(name)
			if (index > -1) {
				this.selectedFruitsByCategory[category].splice(index, 1)
			}
		},
		clearAll() {
			this.selectedFruitsByCategory = {}
			uni.showToast({
				title: '已清空选择',
				icon: 'success'
			})
		},
		copyToClipboard() {
			if (!this.hasSelectedFruits) {
				uni.showToast({
					title: '请先选择水果',
					icon: 'none'
				})
				return
			}

			const content = Object.entries(this.selectedFruitsByCategory)
				.filter(([_, fruits]) => fruits.length > 0)
				.map(([category, fruits]) => `${category}：${fruits.join('，')}`)
				.join('\n\n')

			uni.setClipboardData({
				data: content,
				success: () => {
					uni.showToast({
						title: '已复制水果名称',
						icon: 'success'
					})
				},
				fail: () => {
					uni.showToast({
						title: '复制失败',
						icon: 'none'
					})
				}
			})
		},
	}
}
</script>

<style lang="scss">
.container {
	padding: 20px;
	
	.tab-bar {
		display: flex;
		margin-bottom: 20px;
		border-bottom: 1px solid #eee;
		overflow-x: auto;
		white-space: nowrap;
		.tab-item {
			padding: 16rpx 32rpx;
			font-size: 28rpx;
			color: #666;
			display: inline-block;
			&.active {
				color: #007AFF;
				border-bottom: 2px solid #007AFF;
			}
		}
	}
	
	.header {
		display: flex;
		align-items: flex-end;
		flex-direction: column;
		margin-bottom: 20px;
		
		.action-text {
			font-size: 14px;
			color: #ff4d4f;
			text-align: right;
			margin-top: 10px;
		}
		
		.btn-group {
			display: flex;
			gap: 10px;
			position: relative;
			
			.action-btn {
				padding: 1px 10px;
				border-radius: 8px;
				font-size: 16px;
				border: none;
				position: relative;
				
				&.copy-btn {
					background-color: #4CAF50;
					color: #ffffff;
					
					&:active {
						background-color: #45a049;
					}
					
					.click-hand {
						position: absolute;
						right: -30px;
						top: 50%;
						transform: translateY(-50%);
						width: 24px;
						height: 24px;
						background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="%23ffffff"><path d="M7.5 2.5c-.83 0-1.5.67-1.5 1.5v2c0 .83.67 1.5 1.5 1.5s1.5-.67 1.5-1.5V4c0-.83-.67-1.5-1.5-1.5zm9 0c-.83 0-1.5.67-1.5 1.5v2c0 .83.67 1.5 1.5 1.5s1.5-.67 1.5-1.5V4c0-.83-.67-1.5-1.5-1.5zM12 2c-.83 0-1.5.67-1.5 1.5v2c0 .83.67 1.5 1.5 1.5s1.5-.67 1.5-1.5V3.5c0-.83-.67-1.5-1.5-1.5zM7.5 8c-.83 0-1.5.67-1.5 1.5v2c0 .83.67 1.5 1.5 1.5s1.5-.67 1.5-1.5v-2c0-.83-.67-1.5-1.5-1.5zm9 0c-.83 0-1.5.67-1.5 1.5v2c0 .83.67 1.5 1.5 1.5s1.5-.67 1.5-1.5v-2c0-.83-.67-1.5-1.5-1.5zM12 8c-.83 0-1.5.67-1.5 1.5v2c0 .83.67 1.5 1.5 1.5s1.5-.67 1.5-1.5v-2c0-.83-.67-1.5-1.5-1.5zM7.5 13.5c-.83 0-1.5.67-1.5 1.5v2c0 .83.67 1.5 1.5 1.5s1.5-.67 1.5-1.5v-2c0-.83-.67-1.5-1.5-1.5zm9 0c-.83 0-1.5.67-1.5 1.5v2c0 .83.67 1.5 1.5 1.5s1.5-.67 1.5-1.5v-2c0-.83-.67-1.5-1.5-1.5zM12 13.5c-.83 0-1.5.67-1.5 1.5v2c0 .83.67 1.5 1.5 1.5s1.5-.67 1.5-1.5v-2c0-.83-.67-1.5-1.5-1.5z"/></svg>');
						background-size: contain;
						background-repeat: no-repeat;
						animation: clickHand 1.5s infinite;
						opacity: 0.8;
					}
				}
				
				&.export-btn {
					background-color: #007AFF;
					color: #ffffff;
					
					&:active {
						background-color: #0056b3;
					}
				}
			}
		}
	}
	
	@keyframes clickHand {
		0% {
			transform: translateY(-50%) scale(1);
			opacity: 0.8;
		}
		50% {
			transform: translateY(-50%) scale(1.2);
			opacity: 1;
		}
		100% {
			transform: translateY(-50%) scale(1);
			opacity: 0.8;
		}
	}
	
	.content {
		display: flex;
		gap: 20px;
		height: calc(100vh - 150px);
		
		.fruit-list {
			flex: 1;
			background-color: #ffffff;
			border-radius: 12px;
			padding: 15px;
			box-shadow: 0 2px 12px rgba(0, 0, 0, 0.1);
			overflow-y: auto;
			
			.fruit-item {
				padding: 15px;
				margin-bottom: 10px;
				background-color: #f8f8f8;
				border-radius: 8px;
				text-align: center;
				font-size: 16px;
				color: #333;
				transition: all 0.3s;
				
				&.selected {
					background-color: #E0FFFF;
					color: #007AFF;
					font-weight: bold;
				}
				
				&:active {
					transform: scale(0.98);
				}
			}
		}
		
		.selected-list {
			flex: 1;
			background-color: #ffffff;
			border-radius: 12px;
			padding: 15px;
			box-shadow: 0 2px 12px rgba(0, 0, 0, 0.1);
			display: flex;
			flex-direction: column;
			
			.selected-title {
				display: flex;
				justify-content: space-between;
				align-items: center;
				margin-bottom: 15px;
				padding-bottom: 10px;
				border-bottom: 1px solid #eee;
				
				text {
					font-size: 18px;
					font-weight: bold;
					color: #333;
				}
				
				.clear-all {
					color: #ff4d4f;
					font-size: 14px;
					
					&:active {
						opacity: 0.8;
					}
				}
			}
			
			.selected-items {
				flex: 1;
				overflow-y: auto;
				
				.category-group {
					margin-bottom: 15px;
					
					.category-title {
						font-size: 16px;
						font-weight: bold;
						color: #333;
						margin-bottom: 8px;
					}
					
					.category-items {
						display: flex;
						flex-wrap: wrap;
						gap: 8px;
					}
				}
				
				.selected-item {
					display: flex;
					justify-content: space-between;
					align-items: center;
					padding: 8px 12px;
					background-color: #F0FFF0;
					border-radius: 8px;
					
					text {
						font-size: 14px;
						color: #333;
					}
					
					.delete-btn {
						color: #ff4d4f;
						font-size: 18px;
						padding: 0 5px;
						margin-left: 8px;
						
						&:active {
							opacity: 0.8;
						}
					}
				}
				
				.empty-tip {
					text-align: center;
					color: #999;
					padding: 20px;
					font-size: 14px;
				}
			}
		}
	}
}
</style>