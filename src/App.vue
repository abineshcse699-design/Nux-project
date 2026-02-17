<template>
  <main class="container">
    <h1>{{ message }}</h1>

    <TaskForm @add-task="addTask" />

    <h3 v-if="!tasks.length">Add a task to get started</h3>
    <h3 v-else>{{ totalDone }} / {{ tasks.length }} task completed</h3>

    <div v-if="tasks.length" class="button-container1">
      <FilterButton
        :currentFilter="filter"
        filter="all"
        @set-filter="setFilter"
      />
      <FilterButton
        :currentFilter="filter"
        filter="todo"
        @set-filter="setFilter"
      />
      <FilterButton
        :currentFilter="filter"
        filter="done"
        @set-filter="setFilter"
      />
    </div>

    <Tasklist
      :tasks="filteredTasks"
      @toggle-done="toggleDone"
      @remove-task="removeTask"
    />
  </main>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue'
import TaskForm from '@/components/TaskForm.vue'
import Tasklist from '@/components/Tasklist.vue'
import FilterButton from '@/components/FilterButton.vue'

import type { Task, TaskFilter } from '@/types'

const message = ref('Hello World')
const tasks = ref<Task[]>([])
const filter = ref<TaskFilter>('all')

const totalDone = computed(() =>
  tasks.value.reduce((total, task) => (task.done ? total + 1 : total), 0)
)

const filteredTasks = computed(() => {
  switch (filter.value) {
    case 'all':
      return tasks.value
    case 'done':
      return tasks.value.filter(task => task.done)
    case 'todo':
      return tasks.value.filter(task => !task.done)
    default:
      return tasks.value
  }
})

function addTask(newTask: string) {
  tasks.value.push({
    id: crypto.randomUUID(),
    title: newTask,
    done: false
  })
}

function toggleDone(id: string) {
  const task = tasks.value.find(task => task.id === id)
  if (task) task.done = !task.done
}

function removeTask(id: string) {
  tasks.value = tasks.value.filter(task => task.id !== id)
}

function setFilter(value: TaskFilter) {
  filter.value = value
}
</script>

<style>
body {
  margin: 0;
  font-family: Arial, sans-serif;
}

.container {
  width: 80%;
  max-width: 1200px;
  margin: 2rem auto;
  padding: 1rem;
}

h1 {
  text-align: center;
  margin-bottom: 2rem;
}

.button-container{
  width: 200px;
  margin-left: 80%;
}

.button-container1 {
  display: flex;
  justify-content: flex-end;
  gap: 20px;
  margin: 1rem 0;
  flex-wrap: wrap;
}

/* Responsive */
@media (max-width: 768px) {
  .container {
    width: 95%;
  }

  .button-container1 {
    justify-content: center;
  }
  .button-container{
    justify-content: center;
    margin-left: 60%;
  }
}
</style>
