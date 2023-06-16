<script setup>
import {ref} from "vue";
import Header from './components/Header.vue'
import {BarcodeReader, StrichSDK} from "@pixelverse/strichjs-sdk";

const detections = ref([]);

// setup STRICH SDK and BarcodeReader
const licenseKey = '<your license key>';
StrichSDK.initialize(licenseKey)
    .then(() => {
      console.log(`STRICH SDK initialized, version: ${StrichSDK.version()}`);

      // configure the BarcodeReader
      const configuration = {
        selector: '#scanner',
        engine: {
          symbologies: ['code128']
        },
        frameSource: {
          resolution: 'full-hd',
        },
        debug: true
      };
      const barcodeReader = new BarcodeReader(configuration);
      barcodeReader.initialize()
          .then(() => {
            barcodeReader.detected = (newDetections) => {
              detections.value.push(newDetections[0]);
              console.log(`New scan: [${newDetections[0].data}]`);
            };
            barcodeReader.start();
          });
    })
    .catch(err => {
      window.alert(`STRICH SDK initialization failed: ${err.message}`);
    });
</script>

<template>
  <div id="app">
    <header>
      <div class="wrapper">
        <Header/>
      </div>
    </header>
    <div id="scanner">
      <!-- STRICH BarcodeReader instantiated here -->
    </div>
    <main>
      <p>This sample app is based on Vue 2.x</p>
      <ul v-if="detections.length">
        <li v-for="detection in detections">
          {{ detection.data }}
        </li>
      </ul>
      <p v-else>No barcodes scanned yet.</p>
    </main>
  </div>
</template>

<style scoped>
main {
  padding: 20px;
}
</style>
