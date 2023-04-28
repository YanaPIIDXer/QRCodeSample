<script setup lang="ts">
import { ref, onMounted } from "vue";
import JSQR from "jsqr";

const videoElement = ref<HTMLVideoElement | null>(null);

/**
 * カメラ起動
 */
const startCamera = async () => {
  try {
    if (videoElement.value) {
      const stream = await navigator.mediaDevices.getUserMedia({ video: { facingMode: "environment" } });
      videoElement.value.srcObject = stream;
      videoElement.value.play();
    }
  } catch (error) {
    alert("エラー");
    console.error(error);
  }
};

/**
 * カメラのキャプチャ
 */
const capture = () => {
  requestAnimationFrame(capture);
};

onMounted(() => {
  startCamera();
})

</script>

<template>
  <div class="root">
    <video ref="videoElement" @play="capture()" />
  </div>
</template>

<style lang="sass" scoped>
</style>
