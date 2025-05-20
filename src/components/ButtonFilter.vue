<script setup lang="ts">
import type { FilterType } from '@/types';

const props = defineProps<{
    filter: FilterType,
    currentFilter: FilterType
}>();

const emits = defineEmits<{
    setFilter: [filter: FilterType]
}>()
</script>

<template>
    <button :class="{'outline' : currentFilter !== props.filter}" @click="emits('setFilter', props.filter)">{{ props.filter }}</button>
</template>

<style scoped>
button {
    text-transform: capitalize;
}
</style>