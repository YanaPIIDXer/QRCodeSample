<script setup lang="ts">
import { ref, onMounted } from "vue";
import QrCode from "qrcode";

const qrCode = ref("");
const baseData = {
  id: 1,
  name: "Name",
};

onMounted(async () => {
  try {
    qrCode.value = await QrCode.toDataURL(JSON.stringify(baseData));
  } catch (error) {
    alert("エラー");
    console.error(error);
  }
});

</script>

<template>
  <div class="root">
    <img v-if="qrCode" class="qrCode" :src="qrCode" />
    <span v-else>読み込み中</span>
  </div>
</template>

<style lang="sass" scoped>
.root
  .qrCode
    width: 256px
    height: 256px
</style>
