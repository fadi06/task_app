<script lang="ts" setup>
import { computed, ref } from 'vue';
import type { FilterType, Task } from './types';
import TaskForm from './components/taskForm.vue';
import TaskList from './components/TaskList.vue';
import ButtonFilter from './components/ButtonFilter.vue';

const message = ref("Tasks app")
const tasks = ref<Task[]>([])
const filter = ref<FilterType>("all")

const totalDone = computed(() => tasks.value.reduce((total, task) => task.done ? total + 1 : total, 0));
const filterResult = computed(() => {
  switch(filter.value) {
    case "done":
      return tasks.value.filter(task => task.done);
    case "todo":
      return tasks.value.filter(tasks => !tasks.done);
  }

  return tasks.value
});

function addTask(newTask: string) {
  tasks.value.push({
    id: crypto.randomUUID(),
    title: newTask,
    done: false
  })
}

function toggleDone(id: string) {
  const task = tasks.value.find(task => task.id === id);

  if(task) {
    task.done = !task.done
  }
}

function removeTask(id: string) {
  const task = tasks.value.findIndex(task => task.id === id);

  if(task !== -1) {
    tasks.value.splice(task, 1);
  }
}

function setFilter (value: FilterType) {
  filter.value = value;
}
</script>

<template>
  <main>
    <h1>{{ message }}</h1>
    <TaskForm @add-task="addTask" />
    <h3 v-if="tasks.length === 0">Add a task to get started.</h3>
    <h3 v-else> {{ totalDone }} / {{ tasks.length }} tasks completed.</h3>
    <div v-if="tasks.length" class="button-container">
      <ButtonFilter :currentFilter="filter" filter="all" @set-filter="setFilter" />
      <ButtonFilter :currentFilter="filter" filter="todo" @set-filter="setFilter" />
      <ButtonFilter :currentFilter="filter" filter="done" @set-filter="setFilter" />
    </div>
    <TaskList :tasks="filterResult" @toggle-done="toggleDone" @remove-task="removeTask" />
  </main>
</template>

<style>
main {
  max-width: 800px;
  margin: 1rem auto;
}
.button-container {
  display: flex;
  justify-content: flex-end;
}

.button-container {
  display: flex;
  justify-content: end;
  gap: 0.5rem;
}
</style>