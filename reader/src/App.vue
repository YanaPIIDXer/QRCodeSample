<script setup lang="ts">
import { ref, onMounted } from "vue";
import JSQR from "jsqr";

const videoElement = ref<HTMLVideoElement | null>(null);
const canvasElement = ref<HTMLCanvasElement | null>(null);

/**
 * カメラ起動
 */
const startCamera = async () => {
  try {
    if (videoElement.value) {
      const stream = await navigator.mediaDevices.getUserMedia({
        video: { facingMode: "environment" },
      });
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
  if (!videoElement.value || !canvasElement.value) { return; }

  const ctx = canvasElement.value.getContext("2d");
  if (!ctx) { return; }

  canvasElement.value.width = videoElement.value.videoWidth;
  canvasElement.value.height = videoElement.value.videoHeight;
  ctx.drawImage(videoElement.value, 0, 0, videoElement.value.videoWidth, videoElement.value.videoHeight);

  const imageData = ctx.getImageData(0, 0, videoElement.value.videoWidth, videoElement.value.videoHeight);
  const code = JSQR(imageData.data, imageData.width, imageData.height, { inversionAttempts: 'dontInvert' });

  if (code) {
    const result = JSON.parse(code.data);
    console.log(result);
    return;
  }

  requestAnimationFrame(capture);
};

onMounted(() => {
  startCamera();
});
</script>

<template>
  <div class="root">
    <video ref="videoElement" @canplay="capture()" style="display: none" />
    <canvas ref="canvasElement" style="display: none"></canvas>
  </div>
</template>

<style lang="sass" scoped>
</style>
