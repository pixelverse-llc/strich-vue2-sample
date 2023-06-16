<script setup>
import {StrichSDK} from "@pixelverse/strichjs-sdk";
import {ref} from "vue";

const sdkVersion = ref(StrichSDK.version());
</script>

<template>
  <div class="version">
    SDK version: {{ sdkVersion }}
  </div>
</template>

<style scoped>
.version {
  text-align: center;
}
</style>
