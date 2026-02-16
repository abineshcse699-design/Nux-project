<script setup lang="ts">
import type { TaskFilter } from '../types'

const props = defineProps<{
  filter: TaskFilter
  currentFilter: TaskFilter
}>()

const emit = defineEmits<{
  (e: 'set-filter', filter: TaskFilter): void
}>()
</script>

<template>
  <button
    class="secondary"
    @click="emit('set-filter', props.filter)"
    :class="{ contrast: props.currentFilter === props.filter }"
  >
    {{ props.filter }}
  </button>
</template>

<style scoped>
button {
  text-transform: capitalize;
}
</style>
