<script setup lang="ts">
import { ref, onMounted, type VNodeRef, watch } from 'vue'

const video = ref<VNodeRef | undefined>(undefined)
const props = defineProps({
  src: {
    type: String,
    required: true
  },
  currentTime: {
    type: Number
  },
  isPlaying: {
    type: Boolean,
    default: false
  }
})

const emit = defineEmits(['videoDuration', 'update:currentTime', 'update:isPlaying'])

onMounted(() => {
  video.value.onloadeddata = () => {
    emit('videoDuration', video.value.duration)
  }
})

watch(
  () => props.currentTime,
  (n, o) => {
    if (n !== o && !props.isPlaying) {
      video.value.currentTime = n
      emit('update:currentTime', video.value.currentTime)
    }
  }
)

watch(
  () => props.isPlaying,
  (newValue, oldValue) => {
    console.log(newValue, oldValue)
    handlePlayback(oldValue)
  }
)

const handlePlayback = (playingStatus: Boolean) => {
  if (playingStatus) {
    video.value.pause()
    emit('update:isPlaying', false)
  } else {
    video.value.play()
    emit('update:isPlaying', true)
  }
}

const handleTimeUpdate = (e: Event) => {
  const target = e.target as HTMLVideoElement
  emit('update:currentTime', target.currentTime)
}
</script>

<template>
  <div class="video">
    <video :src="props.src" ref="video" muted @timeupdate="handleTimeUpdate"></video>
  </div>
</template>

<style scoped>
.video {
  width: 100%;
  flex-basis: calc(50% - 4px);
}
video {
  width: 100%;
}
</style>
