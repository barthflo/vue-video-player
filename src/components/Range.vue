<script setup lang="ts">

defineProps({
  modelValue: {
    type: Number
  }
})

defineEmits(['update:modelValue'])
</script>

<template>
  <input type="range" :value="modelValue" min="0" @input="$emit('update:modelValue', Number($event.target.value))" />
</template>

<style scoped></style>
