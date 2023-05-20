<template>
  <view>
    <image-cropper id="image-cropper"
                   :limit_move="true"
                   :disable_rotate="true"
                   :disable_ratio="true"
                   :disable_width='true'
                   :disable_height='true'
                   :width="width"
                   :height="height"
                   :imgSrc="src"
                   @load="cropperload"
                   @imageload="loadimage"
                   @tapcut="clickcut"></image-cropper>
 
    <view class='bottom'>
      <button @tap='submit'>确定</button>
      <button @tap='cancel'>取消</button>
    </view>
  </view>
</template>
 
<script>
export default {
  data() {
    return {
      src: '',
      width: 384/1.5, //宽度
      height: 128/1.5, //高度
    }
  },
  onLoad (options) {
    //获取到image-cropper实例
    this.cropper = this.selectComponent('#image-cropper')
    //开始裁剪
    this.src = options.imgSrc
    wx.showLoading({
      title: '加载中',
    })

  },
  methods: {
    cropperload(e) {
      console.log('cropper初始化完成')
    },
    loadimage(e) {
      console.log('图片加载完成', e.detail)
      wx.hideLoading()
      //重置图片角度、缩放、位置
      this.cropper.imgReset()
    },
    clickcut(e) {
      console.log(e.detail)
      //点击裁剪框阅览图片
      wx.previewImage({
        current: e.detail.url, // 当前显示图片的http链接
        urls: [e.detail.url], // 需要预览的图片http链接列表
      })
    },
    //点击提交按钮时触发
    submit() {
      let requestUrl='http://www.amateur.ga:9200/query_crab'
      this.cropper.getImg((obj)=>{
        wx.uploadFile({
        url:requestUrl,
        filePath:obj.url,
        name:'img',
        success(res){
          console.log(res.data)
          let id=JSON.parse(res.data).id
          let address=JSON.parse(res.data).address
          let time=JSON.parse(res.data).time
          let uuid=JSON.parse(res.data).uuid
          getApp().globalData.identifyImg.imgSrc=obj.url
          getApp().globalData.identifyImg.imgInfo={id,address,time,uuid}
          wx.switchTab({
            url:'/pages/identifyImagePage/identifyImagePage'
          })
        }
      })
      })
      
      // this.cropper.getImg((obj) => {
      //   wx.navigateTo({
      //      //跳回pageA页面，将裁剪好的图片显示在pageA的image标签上
      //     url: '../yewu/zhinengmenjin?src=' + obj.url,
      //   })
      //   console.log('obj', obj)
      // })
    },
    cancel() {
      wx.navigateBack({
        delta: -1,
      })
    },
  },
}
</script>
 
<style scoped>
.bottom {
  position: absolute;
  width: 100%;
  bottom: 50rpx;
  display: flex;
  z-index: 10;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
  height: 210rpx;
}
button {
  font-size: 27rpx;
  z-index: 2;
  padding: 0 20rpx;
  height: 60rpx;
  min-width: 70rpx;
  margin: 0 4rpx;
}
</style>