<template>
  <main>
    <!-- heading -->
    <header>
      <img src="./assets//pinia-logo.svg" alt="logo">
      <h1>{{ taskStore.name }} Tasks</h1>
    </header>

    <div class="new-task-form">
      <TaskForm />
    </div>

    <nav class="filter">
      <button @click="filter = 'all'">All tasks</button>
      <button @click="filter = 'favs'">Fav tasks</button>
    </nav>

    <div class="loading" v-if="taskStore.isLoading">Loading tasks...</div>

    <div class="task-list" v-if="filter === 'all'">
      <p>You have {{ taskStore.totalCount }} tasks left to do</p>
      <div v-for="task in taskStore.tasks">
        <TaskDetail :task="task" />
      </div>
    </div>
    <div class="task-list" v-if="filter === 'favs'">
      <p>You have {{ taskStore.favCount }} fav tasks left to do</p>
      <div v-for="task in taskStore.favs ">
        <TaskDetail :task="task" />
      </div>
    </div>

    <button @click="taskStore.$reset">Reset state</button>

  </main>
</template>

<script setup>
import { useTaskStore } from './stores/TaskStore'
import TaskDetail from './components/TaskDetail.vue'
import TaskForm from './components/TaskForm.vue'
import { ref } from 'vue';
import { storeToRefs } from 'pinia';

const taskStore = useTaskStore()

const { tasks, isLoading, favs, totalCount, favCount } = storeToRefs(taskStore)

taskStore.getTasks()
const filter = ref('all')
</script>
