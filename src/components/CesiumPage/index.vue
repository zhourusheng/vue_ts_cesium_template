<template>
  <div id="cesium-viewer"></div>
</template>

<script setup lang="ts">
import { onMounted } from 'vue'
import * as Cesium from 'cesium'

onMounted(() => {
  initMap()
})

function initMap() {
  new Cesium.Viewer('cesium-viewer')
}
</script>

<style>
#cesium-viewer {
  width: 100%;
  height: 100%;
  overflow: hidden;
}
</style>
