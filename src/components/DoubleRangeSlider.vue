<script setup lang="ts">
import { ref, watch, onMounted } from 'vue'
const props = defineProps({
  maxRange: {
    type: Number,
    default: 100
  },
  step: {
    type: Number,
    default: 0.1
  },
  in: {
    type: Number,
    default: 0
  },
  out: {
    type: Number,
    default: 10
  },
  current: {
    type: Number,
    default: 0
  },
  selectionColour: {
    type: String,
    default: 'black'
  },
  fps: {
    type: Number,
    default: 23.98
  }
})

const emit = defineEmits(['update:in', 'update:out', 'update:current'])
const selectedFill = ref(
  `background: linear-gradient(to right, ${props.selectionColour} 0%, ${props.selectionColour} 100%)`
)
const min = ref(props.in)
const max = ref(props.out)

onMounted(() => {
  updateValuesIn(props.in)
  updateValuesOut(props.out)
})


const updateValuesIn = (value: number) => {
  if (value > Number(max.value)) {
    max.value = value
  }
  min.value = value

  selectedFill.value = `background: linear-gradient(to right,  #c6c6c6 ${
    (min.value / props.maxRange) * 100
  }%, ${props.selectionColour} ${(min.value / props.maxRange) * 100}%, ${props.selectionColour} ${
    (Number(max.value) / props.maxRange) * 100
  }%,  #c6c6c6 ${(Number(max.value) / props.maxRange) * 100}%)`

  emit('update:in', min.value)
  emit('update:out', Number(max.value))
  emit('update:current', value / props.fps)
}

const updateValuesOut = (value: number) => {
  if (value < Number(min.value)) {
    min.value = value
  }
  max.value = value

  selectedFill.value = `background: linear-gradient(to right,  #c6c6c6 ${
    (Number(min.value) / props.maxRange) * 100
  }%, ${props.selectionColour} ${(Number(min.value) / props.maxRange) * 100}%, ${
    props.selectionColour
  } ${(max.value / props.maxRange) * 100}%,  #c6c6c6 ${(max.value / props.maxRange) * 100}%)`
  emit('update:out', value)
  emit('update:in', Number(min.value))
  emit('update:current', value / props.fps)
}

const updateValuesCurrent = (value: number) => {
  emit('update:current', value / props.fps)
}

watch(
  () => props.in,
  (n, _o) => {
    updateValuesIn(n)
  }
)

watch(
  () => props.out,
  (n, _o) => {
    updateValuesOut(n)
  }
)

const emitFrom = (e: Event) => {
  const target = e.target as HTMLInputElement
  const value = Number(target.value)
  updateValuesIn(value)
}

const emitTo = (e: Event) => {
  const target = e.target as HTMLInputElement
  const value = Number(target.value)

  updateValuesOut(value)
}

const emitCurrent = (e: Event) => {
  const target = e.target as HTMLInputElement
  const value = Number(target.value)

  updateValuesCurrent(value)
}
</script>

<template>
  <div class="range_container">
    <input
      type="range"
      :step="step"
      min="0"
      :max="props.maxRange"
      :style="selectedFill"
      :value="props.in"
      @input="emitFrom"
    />
    <input
      type="range"
      :step="step"
      min="0"
      :max="props.maxRange"
      :value="props.out"
      @input="emitTo"
    />
    <input
      id="timelineTrack"
      type="range"
      :step="step"
      min="0"
      :max="props.maxRange"
      :value="props.current * fps"
      @input="emitCurrent"
      
    />
  </div>
</template>

<style scoped lang="scss">
.range_container {
  position: relative;
  width: 100%;
  height: 50px;
  display: flex;
  align-items: center;

  input[type='range'] {
    position: absolute;
    width: 100%;
    --webkit-appearance: none;
    appearance: none;
    height: 2px;
    pointer-events: none;
    background: #c6c6c6;

    &::-webkit-slider-thumb {
      -webkit-appearance: none;
      pointer-events: all;
      width: 8px;
      height: 24px;
      background: #ffffff;
      box-shadow: 0 0 0 1px #c6c6c6;
      cursor: pointer;

      &:hover {
        background-color: #f7f7f7;
      }

      &:active {
        box-shadow:
          inset 0 0 1px #c6c6c6,
          0 0 3px #c6c6c6;
        -webkit-box-shadow:
          inset 0 0 1px #c6c6c6,
          0 0 3px #c6c6c6;
      }
    }

    &::-moz-range-thumb {
      -webkit-appearance: none;
      pointer-events: all;
      width: 4px;
      height: 24px;
      background: #ffffff;
      box-shadow: 0 0 0 1px #c6c6c6;
      cursor: pointer;
    }

    &:nth-child(1), &:nth-child(2) {
      z-index: 1;
      margin-top: 1px;
    }
    &:nth-child(2){
      height: 0;
    }
    &#timelineTrack {
      z-index:10;
      background: transparent;
    }
    &#timelineTrack::-webkit-slider-thumb {
      height:12px;
      width:1px;
      
      box-shadow: 0px 0px 5px 1px transparent;
     background:black;
   
    }
  }

  .values {
    display: flex;
    height: 50px;
    align-items: end;
    justify-content: space-between;
  }
}
</style>
