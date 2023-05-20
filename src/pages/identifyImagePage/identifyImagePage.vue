<template>
  <view>
    <!-- 导航栏 -->
    <view
      class="flex justify-between align-center solid-bottom text-xxl padding bg-white"
    >
      <text class=" cuIcon-file text-black"></text>
      <image src="/static/logo.png" mode="widthFix" style="width:44rpx"></image>
      <text class=" cuIcon-settings text-black"></text>
    </view>
    <!-- 主体 -->
    <view class="text-center margin-bottom">
      <!-- 添加图片 -->
      <view
        class="cu-card bg-black  margin-lr-xl margin-tb shadow"
        style="border-radius:10rpx"
        @click="toCropper"
        v-if="!imgSrc"
      >
        <view class="text-center text-white">
          <view class=" text-xsl padding">
            <text class="cuIcon-add"></text>
          </view>
          <view class="padding">添加图片</view>
        </view>
      </view>
      <!-- 结果展示 -->
      <view v-else>
        <view
          class="cu-card bg-black solid margin-lr-xl margin-tb shadow"
          style="border-radius:10rpx"
          @click="toCropper"
        >
          <image :src="imgSrc" mode="widthFix" style="width:100%"></image>
          <view class="text-center text-white">
            <button
              class="cu-btn icon block  margin round line-white"
              @click.stop="toCropper"
            >
              <text class="cuIcon-add"></text>
            </button>
          </view>
        </view>
        <view> </view>
      </view>
    </view>
    <!-- 历史记录 -->
    <view class="padding flex align-center solids-top"
      ><text class="cuIcon-sort margin-right-sm" style="font-size:44rpx"></text>
      识别信息
    </view>
    <view class="text-center">
      <canvas
      id="qrcode"
      canvas-id="qrcode"
      style="width: 354px;height: 354px;"
    />
    </view>
    
    <!-- <view class="cu-list menu-avatar">

    </view> -->
  </view>
</template>

<script>
import uQRCode from '@/components/uqrcode/common/uqrcode.js'
export default {
  data() {
    return {
      imgSrc: "",
      imgInfo: "",
    };
  },
  onShow(){
    console.log(getApp().globalData.identifyImg)
    if (getApp().globalData.identifyImg.imgSrc){
      this.imgSrc = getApp().globalData.identifyImg.imgSrc;
      this.imgInfo = getApp().globalData.identifyImg.imgInfo;
      uQRCode.make({
        canvasId: 'qrcode',
        componentInstance: this,
        size: 128,
        margin: 10,
        text: 'http://www.amateur.ga:9200/index?uuid='+this.imgInfo.uuid,
        backgroundColor: '#ffffff',
        foregroundColor: '#000000',
        fileType: 'png',
        errorCorrectLevel: uQRCode.errorCorrectLevel.H
    })
    .then(res => {
        console.log(res)
    })
    }
  },
  methods: {
    toCropper() {
      wx.chooseImage({
        count: 1,
        sizeType: ["compressed"],
        sourceType: ["album"],
        success: function(res) {
          const src = res.tempFilePaths[0];
          wx.navigateTo({
            url: "/pages/cropIdentifyImagePage/cropIdentifyImagePage?imgSrc=" + src,
          });
        },
      });
    },
  },
};
</script>

<style>
page {
  background: #fff;
}
.index-header {
  background: #f8f8f8;
  height: 80rpx;
}
.content {
  background: #f8f8f8;
  height: 100%;
  width: 100%;
  text-align: center;
  display: flex;
  justify-content: center;
  align-items: center;
}
.head {
  overflow: hidden;
  border-radius: 100%;
  width: 200rpx;
  height: 200rpx;
  border: 4rpx solid #f8f8f8;
  line-height: 200rpx;
  color: #f8f8f8;
}
</style>
