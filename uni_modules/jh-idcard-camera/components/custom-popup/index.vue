<template>
  <view class="mark" v-if="visible">
    <view class="content" >
      <slot></slot>
    </view>
  </view>
</template>

<script setup>
import { ref, watch, defineExpose, defineEmits } from "vue";

const emits = defineEmits(['change']);
const visible = ref(false);

watch(
  () => visible.value,
  (val) => {
    emits('change', val);
  },
  {
    immediate: false
  }
)

const open = () => {
  visible.value = true;
}

const close = () => {
  visible.value = false;
}

defineExpose({
  open,
  close
});
</script>


<style scoped>
.mark {
  position: fixed;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.7);
  left: 0;
  bottom: 0;
  top: 0;
  right: 0;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
