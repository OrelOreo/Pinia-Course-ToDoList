<template>
  <main>
    <!-- heading -->
    <header>
      <img src="./assets/logo-pinia.svg" alt="pinia logo">
      <h1>Pinia Tasks</h1>
    </header>

    <!-- new task form -->
    <div class="new-task-form">
      <taskForm />
    </div>

    <!-- filter -->
    <nav class="filter">
      <button @click="filter = 'all'">All tasks</button>
      <button @click="filter = 'favs'">Favs tasks</button>
    </nav>

    <!-- loading -->
    <div class="loading" v-if="taskStore.loading">Loading tasks...</div>

    <!-- task list -->
    <div v-if="filter === 'all'" class="task-list">
      <p>You have {{ taskStore.totalCount }} tasks left to do !</p>
      <div :key="task.id" v-for="task in taskStore.tasks">
        <taskDetails :task="task" />
      </div>
    </div>
    <!-- task list -->
    <div v-if="filter === 'favs'" class="task-list">
      <p>You have {{ taskStore.favCount }} favs left to do !</p>
      <div :key="task.id" v-for="task in taskStore.favs">
        <taskDetails :task="task" />
      </div>
    </div>
    <button @click="taskStore.$reset">reset state</button>
  </main>
</template>

<script>
import { ref } from "vue"
import taskDetails from "./components/taskDetails.vue"
import taskForm from "./components/taskForm.vue"
import { useTaskStore } from "./stores/TaskStore"

export default {
  components: { taskDetails, taskForm },
  setup() {
    const taskStore = useTaskStore()
    
    // fetch tasks
    taskStore.getTasks()

    const filter = ref('all')

    return { taskStore, filter }
  }
}
</script>

