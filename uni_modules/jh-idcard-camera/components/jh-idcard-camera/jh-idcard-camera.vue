<template>
  <custom-popup ref="photographRef"  @change="changePopup">
    <view class="popup-content photographCenter" :style="'height: 100vh;box-sizing: border-box;border-radius:0'">
      <image class="back-icon" src="../../static/camera_back.png" @click="photographRef?.close()"></image>
      <view class="waper flex-align-center">
        <view class="noticeTXT">请将身份证{{ type === 'front' ? '人像' : '国徽' }}面放入框内</view>
        <camera v-if="flagShowCamera" mode="normal" device-position="back" flash="auto" @error="error" @initdone="initdone" style="width: 632rpx; height: 400rpx">
         <cover-view class="controls">
            <cover-image v-if="type === 'front'" v-show="coverImgFlag" class="img" src="../../static/camera_module_front.png" />
            <cover-image v-else v-show="coverImgFlag" class="img" src="../../static/camera_module_side.png" />
          </cover-view>
        </camera>
      </view>
      <view class="distinguish_click">
        <view class="distinguish_click_item" @click="openAlbum">
          <image class="image-icon" src="../../static/camera_image.png"></image>
          <text class="text">相册</text>
        </view>
        <image class="take-photo-icon" @click="distinguish" src="../../static/camera_take_photo.png"></image>
      </view>
    </view>
  </custom-popup>
</template>

<script setup>
  import { ref, defineExpose, defineEmits, defineProps } from "vue";
  import CustomPopup from "../custom-popup/index.vue";
  
  const props = defineProps({
    quality: {
      type: String,
      default: 'normal' // high (high quality), normal (normal quality), low (low quality)
    },
    sizeType: {
      type: Array,
      default: ['original', 'compressed']
    }
  })
  const emits = defineEmits(['getData', 'chooseImageFail', 'cameraInitDone', 'cameraInitError', 'takePhotoError']);
  
  const photographRef = ref();
  const coverImgFlag = ref(true);
  const pic = ref('');
  const flagShowCamera = ref(false);
  const type = ref('front');
  
  const error = (e) => {
    emits('cameraInitError', e);
  }
  
  const initdone = (e) => {
    emits('cameraInitDone', e);
  }
  
  const isBase64Img = (pathUrl) => {
    uni.getFileSystemManager().readFile({
      filePath: pathUrl,
      encoding: 'base64',
      success: res => {
        photographRef?.value.close();
        emits('getData', {
          type: type.value,
          base64: res.data,
          url: pathUrl
        })
      },
      complete: res => {
        uni.hideLoading();
      }
    })
  }
  
  const openAlbum = () => {
    uni.chooseImage({
      count: 1,
      sizeType: props.sizeType, 
      sourceType: ['album'],
      success: res => {
        pic.value = res.tempFilePaths[0]
        uni.showLoading({
          title: '保存中'
        });
        isBase64Img(res.tempFilePaths[0])
      },
      fail: err => {
        emit('chooseImageFail', err);
      }
    })
  }
  
  const distinguish = () => {
    const ctx = uni.createCameraContext()
    ctx.takePhoto({
      quality: props.quality,
      success: (res) => {
        pic.value = res.tempImagePath
        uni.showLoading({
          title: '保存中'
        });
        isBase64Img(res.tempImagePath)
      },
      fail: err => {
        emits('takePhotoError', err);
      }
    })
  }
  
  const changePopup = (e) => {
    flagShowCamera.value = e;
  }
  
  const open = (t="front") => {
    type.value = t;
    photographRef?.value.open();
  }
  
  const close = () => {
    photographRef?.value.close();
  }
  
  defineExpose({
    open,
    close
  })
  
</script>


<style scoped>
.waper {
  width: 100%;
  height: 40vh;
  align-items: center;
  flex-direction: column;
}

.controls {
  position: relative;
  top: 0;
  display: flex;
  align-items: center;

}

.controls .img {
  height: 400rpx;
}

.noticeTXT {
  text-align: center;
  margin-bottom: 100rpx;
  color: #fff;
  font-size: 40rpx;
  margin-top: 30rpx;
}

.takePhoto {
  width: 90%;
  margin: 0 auto
}

.flex-align-center {
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.bg-brown {
  color: #fff;
  background: linear-gradient(153deg, rgba(225, 164, 70, 1) 0%, rgba(195, 144, 65, 1) 100%);
}

.distinguish_click {
  position: absolute;
  bottom: 4%;
  left: 0;
  right: 0;
  display: flex;
  justify-content: center;
  align-items: center;
}

.distinguish_click_item {
  position: absolute;
  left: 6%;
  display: flex;
  flex-direction: column;
}

.distinguish_click_item .text {
  color: #fff;
  margin-top: 4rpx;
}

.back-icon {
  margin-top: 20rpx;
  width: 48rpx;
  height: 48rpx;
}

.image-icon {
  width: 60rpx;
  height: 60rpx;
}

.take-photo-icon {
  width: 160rpx;
  height: 160rpx;
}
</style>
