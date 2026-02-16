<template>


    <h1>{{ message }}</h1>
    
  <TaskForm @add-task="addTask"></TaskForm>
  <h3 v-if="!tasks.length">Add a task to get started</h3>
  <h3 v-else> {{ totalDone }} /{{ tasks.length }} task completed</h3>
  <div v-if="tasks.length" class="button-container1">
   <FilterButton :currentFilter="filter" filter="all" @set-filter="setFilter"/>
   <FilterButton :current-filter="filter" filter="todo" @set-filter="setFilter"/>
   <FilterButton filter="done" @set-filter="setFilter"/>
<!-- 
    <button class="secondary" @click="filter='all'">All</button>
    <button class="secondary" @click="filter='todo'">Todo</button>
    <button class="secondary" @click="filter='done'">done</button> -->

  </div>

<Tasklist :tasks="filteredTasks" @-toggle-done="toggleDone" @remove-task="removeTask" />

</template>

<script setup lang="ts">

import {computed, ref} from 'vue';
import TaskForm from '@/components/TaskForm.vue';
import Tasklist from '@/components/Tasklist.vue';
import { Task, TaskFilter } from './types';
import FilterButton from './components/FilterButton.vue';

const message =ref("hello world")
const tasks=ref<Task[]>([]);
const filter =ref<TaskFilter>("all");

const totalDone=computed(()=>tasks
.value
.reduce((total,task)=>task.done?total+1:total,0));

const filteredTasks=computed(()=>{
switch(filter.value){
  case"all":
  return tasks.value;
  case"done":
  return tasks.value.filter((tasks)=> tasks.done);
  case"todo":
  return tasks.value.filter((tasks)=>!tasks.done);
}
return tasks.value;
});


function addTask(newTask:string){
  tasks.value.push({
  
    id:crypto.randomUUID(),
    title:newTask,
    done:false,

  });
}

function toggleDone(id:string){
  const task = tasks.value.find((task)=>task.id===id);
  if(task){
    task.done=!task.done;
  }
}

function removeTask(id:String){
  const index = tasks.value.findIndex((task)=> task.id===id);
  if(index!==1){
    tasks.value.splice(index,1);
  }
}


function setFilter(value: TaskFilter){
  filter.value=value;
}

</script>

<style >
main{
    max-width: 800px;
    margin:1rem-auto;
}
.button-container{
  display: flex;
  /* justify-content: end; */
  gap: 20px;
  width: 200px;
  margin-left: 85%;
}
.button-container1{
  display: flex;
  justify-content: end;
  gap: 20px;
  margin-right: 20px;
}


</style>