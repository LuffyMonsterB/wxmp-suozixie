<template>
  <view>
    <!-- 导航栏 -->
    <view
      class="flex justify-between align-center solid-bottom text-xxl padding"
    >
      <text class=" cuIcon-file text-black"></text>
      <image src="/static/logo.png" mode="widthFix" style="width:44rpx"></image>
      <text class=" cuIcon-settings text-black"></text>
    </view>
    <!-- 上传标注图片模块 -->
    <view>
      <view
        class="cu-card bg-blue padding margin-lr-xl margin-top shadow"
        style="border-radius: 30rpx;"
      >
        <view class="grid col-3 grid-square flex-sub align-center">
          <view
            class="bg-img"
            v-for="(item, index) in imgList"
            :key="index"
            @click="inputImgInfo"
            :data-index="index"
          > 
            <image
              :src="item.imgPath"
              mode="aspectFill"
              style="border-radius:20rpx;"
            ></image>
            <view class="cu-capsule round">
              <view class="cu-tag bg-red">
                <view @click.stop="inputImgInfo" :data-index="index">
                  <text class="line-red" v-show="!item.isEdited">点击录入</text>
                  <text class="line-green" v-show="item.isEdited"
                    >录入完成</text
                  >
                </view>
                <view @click.stop="delImg" :data-index="index"
                  ><text class="cuIcon-close margin-left-xs"></text
                ></view>
              </view>
            </view>
          </view>
          <view
            style="border-radius:20rpx;border:5rpx solid white"
            @click="chooseImg"
            v-if="imgList.length < 9"
          >
            <text class="cuIcon-cameraadd" style="color:#fff"></text>
          </view>
        </view>
      </view>
      <button
        class="margin-lr-xl margin-tb cu-btn lines-blue shadow cuIcon-upload block round"
        @click="uploadImg"
      >
        上传 {{ imgList.length }}/9
      </button>
    </view>
    <!-- 统计数据展示 -->
    <view class="padding flex align-center solids-top margin-top"
      ><text class="cuIcon-edit margin-right-sm" style="font-size:44rpx"></text>
      数据统计
    </view>
    <view class="flex padding-lr">
      <view class="flex-sub margin-lr-sm">
        <view
          class="cu-card shadow bg-white padding-tb-xs padding-lr"
          style="border-radius:20rpx;"
        >
          <view class="text-blue text-xl text-bold">
            <text class="cuIcon-activityfill"></text>
          </view>
          <view class="flex align-center justify-between margin-tb-sm">
            <view class="flex align-center ">
              <view class="text-xl text-bold margin-right-xs"
                ><text>80</text></view
              >
              <view class="text-sm"><text>只</text></view>
            </view>
            <view class=" text-sm">
              <text class="text-grey">梭子蟹数量</text>
            </view>
          </view>
        </view>
      </view>
      <view class="flex-sub margin-lr-sm ">
        <view
          class="cu-card shadow bg-white padding-tb-xs padding-lr"
          style="border-radius:20rpx;"
        >
          <view class="text-yellow text-xl text-bold">
            <text class="cuIcon-favorfill"></text>
          </view>
          <view class="flex align-center justify-between margin-tb-sm">
            <view class="flex align-center ">
              <view class="text-xl text-bold margin-right-xs"
                ><text>80</text></view
              >
              <view class="text-sm"><text>只</text></view>
            </view>
            <view class=" text-sm">
              <text class="text-grey">梭子蟹数量</text>
            </view>
          </view>
        </view>
      </view>
    </view>
    <view class="padding-lr margin-lr-sm">
      <view
        class="cu-card bg-white block margin-tb shadow padding-tb-xs padding-lr"
        style="height:120rpx ;border-radius:20rpx;"
      >
        <view>
          <view class="flex align-center margin-bottom-sm">
            <text
              class="cuIcon-appreciatefill text-green margin-right-xs"
            ></text>
            <text>健康比例：80%</text>
          </view>
          <view>
            <view class="cu-progress round sm striped active">
              <view class="bg-green" style="width:80%;"></view>
            </view>
          </view>
        </view>
      </view>
    </view>
    <view class="padding-lr margin-lr-sm">
      <view
        class="cu-card bg-white block margin-tb shadow padding-tb-xs padding-lr"
        style="height:120rpx ;border-radius:20rpx;"
      >
        <view>
          <view class="flex align-center margin-bottom-sm">
            <text class="cuIcon-tagfill text-cyan margin-right-xs"></text>
            <text>公蟹比例：55%</text>
          </view>
          <view>
            <view class="cu-progress round sm striped active">
              <view class="bg-cyan" style="width:55%;"></view>
            </view>
          </view>
        </view>
      </view>
    </view>
    <!-- 梭子蟹信息录入弹窗 -->
    <view :class="['cu-modal', showEditInfoModal ? 'show' : '']">
      <view class="cu-dialog">
        <view
          class="bg-img"
          :style="{
            'background-image':
              'url(' + imgList[inputingImgIndex].imgPath + ')',
            height: '300rpx',
          }"
        >
          <view class="cu-bar justify-end text-white">
            <view class="action" @click="showEditInfoModal = false">
              <text class="cuIcon-close "></text>
            </view>
          </view>
        </view>
        <view class="cu-bar bg-white flex justify-center">
          <form>
            <view
              class="cu-form-group margin-top flex justify-start align-center flex-wrap"
            >
              <view class=" basis-xs margin-xs"><view>种类</view></view>
              <picker
                @change="kindPickerChange"
                :range="inputingImgFormData.kindPickers"
              >
                <view class="picker flex justify-center">
                  {{
                    imgList[inputingImgIndex].info.kind
                      ? imgList[inputingImgIndex].info.kind
                      : inputingImgFormData.kindPickers[0]
                  }}
                </view>
              </picker>
            </view>
            <view
              class="cu-form-group  flex justify-start align-center flex-wrap"
            >
              <view class=" basis-xs margin-xs"><view>编号</view></view>
              <input
                placeholder="请输入数字或字母"
                @change="idInputChange"
                :value="imgList[inputingImgIndex].info.id"
              />
            </view>
            <!-- <view
              class="cu-form-group  flex justify-between align-center flex-wrap"
            >
              <view class=" basis-xs margin-xs"><view>性别</view> </view>
              {{ inputingImgForm.gender }}
              <switch
                class="switch-sex"
                checked
                @change="genderSwitchChange"
              ></switch>
            </view> -->
            <view
              class="cu-form-group  flex justify-start align-center flex-wrap"
            >
              <view class=" basis-xs margin-xs"><view>产地</view></view>
              <input
                placeholder="请精确到实验室"
                @change="addressInputChange"
                :value="imgList[inputingImgIndex].info.address"
              />
            </view>
            <view
              class="cu-form-group  flex justify-start align-center flex-wrap"
            >
              <view class=" basis-xs margin-xs"><view>时间</view></view>
              <input :value="imgList[inputingImgIndex].info.time" disabled />
            </view>
          </form>
        </view>
        <view class="cu-bar bg-white">
          <view
            class="action flex-sub solid-top text-blue padding-sm"
            @click="checkInputInfo"
            >确定</view
          >
        </view>
      </view>
    </view>
    
  </view>
</template>

<script>

let App = getApp();
export default {

  data() {
    return {
      imgList: [],
      showEditInfoModal: false,
      inputingImgIndex: 0,
      inputingImgFormData: {
        kindPickers: ["梭子蟹", "大闸蟹", "青蟹"],
        genders: ["公", "母"],
      },
    };
  },
  onLoad() {
    this.imgList = App.globalData.uploadImgList;
  },
  methods: {
    uploadImg() {
      if (this.imgList.length != 0) {
        // 检查所有图片信息是否输入
        for (let index in this.imgList) {
          if (!this.imgList[index].isEdited) {
            wx.showToast({
              title: "请录入图片信息",
              icon: "error",
              duration: 1000,
            });
            return;
          }
        }
        //上传图片
        for (let index in this.imgList) {
          wx.uploadFile({
            url: "http://www.amateur.ga:9200/insert_crab",
            filePath: this.imgList[index].imgPath,
            name: "img",
            formData: {
              ...this.imgList[index].info,
            },
            success(res) {
              imgId.push(JSON.parse(res.data).uuid);
            },
          });
        }
        
        
            wx.showToast({
          title: "上传成功",
          icon: "success",
          duration: 2000,
        });
      }
    },
    chooseImg() {
      wx.chooseImage({
        count: 9, //默认9
        sizeType: ["compressed"],
        sourceType: ["album"], //从相册选择
        success: (res) => {
          res.tempFilePaths.forEach((item) => {
            //创建梭子蟹对象：图片路径+信息+是否输入
            let info = {
              kind: "",
              id: "",
              address: "",
              time: "",
            };
            this.imgList.push({
              imgPath: item,
              info: info,
              isEdited: false,
            });
          });
        },
      });

    },
    inputImgInfo(e) {
      this.inputingImgIndex = e.currentTarget.dataset.index;
      let nowTime = new Date();
      this.imgList[this.inputingImgIndex].info.time = [
        nowTime.getFullYear(),
        nowTime.getMonth() + 1,
        nowTime.getDate(),
      ].join("-");
      this.imgList[
        this.inputingImgIndex
      ].info.kind = this.inputingImgFormData.kindPickers[0];
      this.showEditInfoModal = true;
    },
    delImg(e) {
      this.imgList.splice(e.currentTarget.dataset.index, 1);
    },
    checkInputInfo() {
      for (let item in this.imgList[this.inputingImgIndex].info) {
        if (!this.imgList[this.inputingImgIndex].info[item]) {
          wx.showToast({
            title: "信息填写不完整",
            icon: "error",
            duration: 1000,
          });
          return;
        }
      }
      this.imgList[this.inputingImgIndex].isEdited = true;
      this.showEditInfoModal = false;
    },
    idInputChange(e) {
      this.imgList[this.inputingImgIndex].info.id = e.detail.value;
    },
    kindPickerChange(e) {
      this.imgList[
        this.inputingImgIndex
      ].info.kind = this.inputingImgFormData.kindPickers[e.detail.value];
    },
    addressInputChange(e) {
      this.imgList[this.inputingImgIndex].info.address = e.detail.value;
    },
    // genderSwitchChange(e) {
    //   if (e.detail.value) {
    //     this.inputingImgForm.gender = this.inputingImgFormData.genders[0];
    //   } else {
    //     this.inputingImgForm.gender = this.inputingImgFormData.genders[1];
    //   }
    // },
  },
};
</script>

<style>
page {
  background: #f8f8f8;
}
</style>
