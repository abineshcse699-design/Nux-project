<script setup lang="ts">
import { ref } from 'vue'

const newTask = ref('')
const error = ref('')

const emit = defineEmits<{
  (e: 'add-task', value: string): void
}>()

function formSubmitted(): void {
  if (newTask.value.trim()) {
    emit('add-task', newTask.value.trim())
    newTask.value = ''
    error.value = ''
  } else {
    error.value = 'Task cannot be empty!'
  }
}
</script>

<template>
  <form @submit.prevent="formSubmitted">
    <label>
      <input
        v-model="newTask"
        :aria-invalid="!!error || undefined"
        @input="error = ''"
      />

      <small v-if="error">
        {{ error }}
      </small>
    </label>

    <div class="button-container">
      <button type="submit">Add</button>
    </div>
  </form>
</template>
