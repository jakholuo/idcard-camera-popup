<template>
	<view>
    <button @click="openFront">打开身份证正面弹窗</button>
    <button @click="openSide">打开身份证反面弹窗</button>
    <jh-idcard-camera ref="cameraPopup" />
	</view>
</template>

<script setup>
  import { ref, onMounted, watch, getCurrentInstance, nextTick, reactive, computed, defineExpose, defineEmits } from "vue";
  import { onLoad, onShareAppMessage, onShareTimeline, onShow, onPullDownRefresh } from "@dcloudio/uni-app";

  const cameraPopup = ref();
  
  const openFront = () => {
    cameraPopup.value?.open('front');
  }
  
  const openSide = () => {
    cameraPopup.value?.open('side');
  }
  
</script>

<style>
</style>
