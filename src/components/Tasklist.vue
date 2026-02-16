<script lang="ts" setup>
import type { Task } from '../types';

const props = defineProps<{
  tasks: Task[]
}>();


const emits = defineEmits<{
    ToggleDone:[id:string];
    removeTask:[id:string];

}>();

</script>

<template>
  <article v-for="task in props.tasks" class="tasks" :key="task.id">
    <label >
        <input  @input="emits('ToggleDone',task.id)" :checked="task.done" type="checkbox">
        

          <span :class="{done:task.done}">{{ task.title }}</span>

    </label>
    <button class="outline" @click="emits('removeTask',task.id)">remove</button>
  
  </article>
</template>


<style>

.done{
    text-decoration: line-through;
}

.tasks{

    display: flex;
    justify-content: space-between;
    align-items: center;

}
</style>
