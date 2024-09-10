<script setup>
import vad from '../src/vad.vue'
import tts from '../src/components/tts.vue'
import audioRecorder from './components/audioRecorder.vue'
import fileUpload from './components/fileUpload.vue'
</script>

<template>
  <div class="parent">
    <div class="div1">
      <p id="header">实时说话检测</p>
    </div>
    <vad/>
    <tts/>
    <audioRecorder/>
    <fileUpload/>
  </div>
</template>

<style scoped>
.div1 {
  grid-area: 1 / 1 / 2 / 3;
  display: flex;
  align-items: flex-end;
}
#header {
  font-size: 56px;
  font-weight:light;
}
</style>
