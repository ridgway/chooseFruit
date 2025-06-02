<template>
	<view>
        <uni-notice-bar show-icon scrollable
					text="所有图形均为压缩不清晰图，不可商用，只做展示，需要者微信交流" />
	<view class="border-container">
		<!-- <view class="border-container-tiele">边框图和水果图都为压缩不清晰图，不可商用</view> -->
	  <!-- 左侧菜单 -->
	  <view class="border-menu">
		<view
		class="menu-item"
		  v-for="(item, idx) in filteredBordersleft"
		  :key="idx"
		  :class="['c-item', {active: idx === activeMenuIdx}]"
		  @tap="selectMenu(idx)"
		>
		  <image :src="item.img" class="menu-img" mode="aspectFit" />
		  <view class="menu-title">{{ item.title }}</view>
		  
		</view>
	  </view>
	  <!-- 右侧内容：两列豆腐块 -->
	  <view class="border-content grid-mode">
		<view
		  v-for="(item, idx) in filteredBorders"
		  :key="idx"
		  class="border-card"
		>
		  <view class="img-stack">
			<image :src="borderMenu[activeMenuIdx].img" class="border-card-img overlay" mode="aspectFill"/>
			<image :src="item.img" class="border-card-img underlay" mode="aspectFill"/>
		  </view>
		  <view class="border-card-title">{{ item.title }}</view>
		</view>
	  </view>
	</view>
    </view>
    
  </template>
  
  <script>
  import { borderMenu } from '@/fdata/biankuangData'

  // 右侧内容可根据实际需求调整
  const borders = [
    { img: '/static/image/1.jpg'},
    { img: '/static/image/2.jpg'},
    { img: '/static/image/3.jpg' },
    { img: '/static/image/4.jpg' },
    { img: '/static/image/5.jpg' }
  ]
  export default {
    data() {
      return {
        borderMenu,
        borders,
        activeMenuIdx: 0,
        borderszhh: []
      }
    },
    computed: {
      filteredBorders() {
        return this.borders.map((item, index) => {
			return {img: item.img, title: this.getImageNameFromUrl(item.img)}
		})
      },
      filteredBordersleft() {
        return this.borderMenu.map((item, index) => {
			return {img: item.img, title: this.getImageNameFromUrl(item.img)}
		})
      },
    },
    methods: {
    //   selectMenu(idx) {
    //     this.activeMenuIdx = idx
    //   },
    async selectMenu(idx) {
    this.activeMenuIdx = idx
    const menuImg = this.borderMenu[idx].img
  },
  // 合成两张图片并转base64
  
	  getImageNameFromUrl(url) {
	    if (!url) return null;
	    
	    // 解码URL以处理可能的编码字符
	    try {
	      url = decodeURI(url);
	    } catch (e) {
	      console.warn('URL解码失败:', e);
	    }
	    
	    // 尝试匹配常见图片扩展名
	    const regex = /(?:\/|\\|^)([^/\\&\?]+\.)(?:jpe?g|png|gif|bmp|webp|svg|tiff?|ico)(?:[\?&#].*)?$/i;
	    const match = url.match(regex);
	    
	    return match ? match[1].slice(0, -1) : null;
	  }
    }
  }
  </script>
  
  <style lang="scss">
  .border-container {
	.border-container-tiele{
	  background-color: rgb(255, 154, 67);
	  font-size: 24rpx;
	  padding: 10rpx;
	  position: absolute;
	  left: 0;
	  top: 0;
	  width: 100%;
	}
    display: flex;
	height: 100vh;
	flex-wrap: wrap;
	justify-content: flex-start;
	align-items: flex-start;
	background: #fafafa;
    .border-menu {
      width: 200rpx;
	  display: flex;
	  height: 100vh;
      background: #f0eded;
      padding-top: 0rpx;
	  flex-direction: column;
      overflow-y: auto;
      .menu-item {
        padding: 18rpx 0;
        text-align: center;
        color: #bbb;
        font-size: 30rpx;
        &.active {
          background: #fff;
          box-shadow: 2rpx 0 8rpx #eee;
        }
        .menu-img {
          width: 80rpx;
          height: 80rpx;
          border-radius: 12rpx;
          background: #eeeeee;
          border: 2rpx solid #411dd127;
        }
        &.active .menu-img {
          border: 2rpx solid #007AFF;
          background: #fff;
        }
		.menu-title{
			font-size: 24rpx;
		}
      }
    }
    .border-content.grid-mode {
	  width: 550rpx;
	  box-sizing: border-box;
      padding: 18rpx 18rpx 18rpx 18rpx;
	  display: flex;
	  justify-content: space-between;
	  align-items: self-start;
	  flex-wrap: wrap;
      overflow-y: auto;
      .border-card {
        background: #fff;
        // border-radius: 18rpx;
        box-shadow: 0 2rpx 12rpx rgba(0,0,0,0.06);
        display: flex;
        align-items: center;
		flex-direction: column;
		width: 250rpx;
		height: 300rpx;
		margin-bottom: 20rpx;
        // padding: 24rpx 12rpx 18rpx 12rpx;
        .img-stack {
          position: relative;
          width: 250rpx;
          height: 250rpx;
          .border-card-img {
            position: absolute;
            left: 0; top: 0;
            width: 250rpx;
            height: 250rpx;
            border-radius: 12rpx;
            object-fit: cover;
          }
          .underlay {
            z-index: 1;
          }
          .overlay {
            z-index: 2;
            opacity: 0.7; // 可调节透明度
            pointer-events: none;
          }
        }
        .border-card-title {
          font-size: 30rpx;
          color: #222;
          font-weight: bold;
          text-align: center;
		  padding: 10rpx;
        }
      }
    }
  }
  </style>
  