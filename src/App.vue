<script lang="ts" setup>
import { computed, ref } from "vue";
import { Task } from "./types/task";
import ListTasks from "./components/ListTasks.vue";

const tasks = ref<Task[]>([]);
const newTaskTitle = ref("");
const filter = ref("all");

let nextTaskId = 1;

const addTask = () => {
  if (newTaskTitle.value.trim() !== "") {
    console.log("Adding task:", newTaskTitle.value);

    tasks.value.push(new Task(nextTaskId++, newTaskTitle.value, false));
  }
};

const deleteTask = (id: number) => {
  tasks.value = tasks.value.filter((task) => task.id !== id);
};

const tasksFiltered = computed(() => {
  if (filter.value === "all") {
    return tasks.value;
  } else if (filter.value === "completed") {
    return tasks.value.filter((task) => task.completed);
  } else if (filter.value === "pending") {
    return tasks.value.filter((task) => !task.completed);
  }
  return tasks.value;
});
</script>

<template>
  <div class="app">
    <h1>Task List</h1>
    <input
      v-model="newTaskTitle"
      placeholder="Enter new task"
      @keyup.enter="addTask"
    />
    <div class="btn-group">
      <button class="button outline" @click="addTask">Add Task</button>
    </div>

    <br />
    <hr />
    <div class="filter" v-if="tasks.length > 0">
      <button class="button outline" @click="filter = 'all'">All</button>
      <button class="button outline" @click="filter = 'completed'">
        Completed
      </button>
      <button class="button outline" @click="filter = 'pending'">
        Pending
      </button>
    </div>
    <ListTasks :tasksFiltered="tasksFiltered" @delete="deleteTask" />
  </div>
</template>

<style>
.app {
  display: flex;
  flex-direction: column;
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
}

.filter {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.btn-group {
  display: flex;
  justify-content: flex-end;
}

button {
  justify-self: end;
  padding: 10px;
  background-color: #42b983;
  color: white;
  border: none;
  cursor: pointer;
}
</style>
