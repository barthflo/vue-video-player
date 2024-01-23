<script setup lang="ts">
import DoubleRangeSlider from '@/components/DoubleRangeSlider.vue'
import InputNumber from '@/components/InputNumber.vue'
import VideoStream from '@/components/VideoStream.vue'
import Button from '@/components/Button.vue'
import { ref } from 'vue'

const inValue = ref(0)
const outValue = ref(1000)
const step = 1
const selectionColour = 'cadetblue'
const maxRangeValue = ref(1000)
const fps = 23.98
const currentTime = ref(0)
const playing = ref(false)

// watch(inValue, (newValue, oldValue) => {
//     if(newValue && newValue  !== oldValue){
//       currentTime.value = newValue / fps

//     }

// })

// watch(outValue, (newValue, oldValue) => {
//   if(newValue && newValue !== oldValue){
//       currentTime.value = newValue

//     }

// })

// watch(outValue, (n, o) => {
//   if(n && n !== o){
//     currentTime.value = n
//   }
// })
</script>

<template>
  <main>
    <div class="video-container">
      <VideoStream
        v-for="n in 4"
        :key="n"
        src="http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/BigBuckBunny.mp4"
        v-model:current-time="currentTime"
        @video-duration="
          (t) => {
            maxRangeValue = Math.round(t * fps)
            outValue = maxRangeValue
          }
        "
        v-model:is-playing="playing"
      />
    </div>
    <Button
      @handleClick="(e) => (playing ? (playing = false) : (playing = true))"
      :label="playing ? 'Pause' : 'Play'"
    />
    Time: {{ Math.round(currentTime) }}s - Frame Number: {{ Math.round(currentTime * fps) }}
    <DoubleRangeSlider
      v-model:in="inValue"
      v-model:out="outValue"
      v-model:current="currentTime"
      :selection-colour="selectionColour"
      :max-range="maxRangeValue"
      :step="step"
    />

    <div class="input_outer_container">
      <InputNumber v-model="inValue" :max-range="maxRangeValue" :step="step" label="Frame in" />
      <InputNumber v-model="outValue" :max-range="maxRangeValue" :step="step" label="Frame out" />
    </div>
  </main>
</template>

<style scoped>
.input_outer_container {
  display: flex;
  justify-content: space-between;
}

.video-container {
  display: flex;
  width: 100%;
  gap: 4px;
  flex-wrap: wrap;
}
</style>
