<template>
	<view>
    <button @click="openFront">打开身份证正面弹窗</button>
    <button @click="openSide">打开身份证反面弹窗</button>
    <jh-idcard-camera ref="cameraPopup" @get-photo="getPhoto" />
	</view>
</template>

<script setup>
  import { ref } from "vue";

  const cameraPopup = ref();
  
  const openFront = () => {
    cameraPopup.value?.open('front');
  }
  
  const openSide = () => {
    cameraPopup.value?.open('side');
  }

  const getPhoto = (data) => {
    const { type, base64, url } = data;
    console.log('正反面类型', type);
    console.log('图片base64', base64);
    console.log('储存在微信本地的临时地址', url);
  }
  
</script>
