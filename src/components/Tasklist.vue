<script setup lang="ts">
import type { Task } from '../types'

const props = defineProps<{
  tasks: Task[]
}>()

const emit = defineEmits<{
  (e: 'toggle-done', id: string): void
  (e: 'remove-task', id: string): void
}>()
</script>

<template>
  <article
    v-for="task in props.tasks"
    :key="task.id"
    class="tasks"
  >
    <label>
      <input
        type="checkbox"
        :checked="task.done"
        @change="emit('toggle-done', task.id)"
      />
      <span :class="{ done: task.done }">
        {{ task.title }}
      </span>
    </label>

    <button
      class="outline"
      @click="emit('remove-task', task.id)"
    >
      remove
    </button>
  </article>
</template>

<style>
.done {
  text-decoration: line-through;
}

.tasks {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>
