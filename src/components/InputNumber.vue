<script setup lang="ts">

defineProps({
  modelValue: {
    type: Number
  },
  step: {
    type: Number
  },
  maxRange : {
    type: Number
  },
  label : {
    type: String
  }
})

defineEmits(['update:modelValue'])
</script>

<template>
  <div class="input_inner_container">
    <label :for="label">{{ label }}</label>
    <input :id="label" type="number" :step="step" :value="modelValue" min="0" max="maxRange" @input="$emit('update:modelValue', Number($event.target?.value))" />

  </div>
</template>

<style scoped lang="scss">
.input_inner_container {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  label {
    font-weight: 500;
    text-transform: uppercase;
    font-size:0.8rem;
  }
  input {
    border-radius:6px;
    padding: 6px 8px;
    border: 2px solid cadetblue;
  }
}
</style>