<script setup lang="ts">

defineProps({
    label : {
        type: String
    }
})
</script>

<template>
    <button @click="$emit('handleClick')">{{label}}</button>
</template>