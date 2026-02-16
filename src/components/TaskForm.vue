<script setup lang="ts">
import { ref } from 'vue'

const newTask = ref<string>('')
const error = ref<string>('')

const emit = defineEmits<{
  (e: 'addTask', value: string): void
}>()

function formSubmitted(): void {
  if (newTask.value.trim()) {
    emit('addTask', newTask.value.trim())
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
    
      <input v-model="newTask" name="newTask" :aria-invalid="!! error || undefined" 
      @input="error=''"
      >

      <small v-if="error" id="invalid-helper">
         {{ error }}
      </small>
    </label>

    <div class="button-container">
      <button type="submit">Add</button>
    </div>
  </form>



  <h3>{{ newTask }}</h3>
</template>
