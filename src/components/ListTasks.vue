<script setup lang="ts">
import { Task } from "../types/task";
import { defineProps } from "vue";

const props = defineProps<{
  tasksFiltered: Task[];
}>();
</script>

<template>
  <div>
    <ul>
      <li class="item" v-for="task in tasksFiltered" :key="task.id">
        {{ task.name }} - {{ task.completed ? "Done" : "Pending" }}
        <button
          class="button-complete"
          v-if="!task.completed"
          @click="task.completed = true"
        >
          Mark as done
        </button>
      </li>
    </ul>
  </div>
</template>

<style scoped>
ul {
  list-style-type: none;
  padding: 0;
}

.button-complete {
  background-color: gray;
}

.item {
  display: flex;
  width: 100%;
  padding: 10px;
  min-height: 70px;
  justify-content: space-between;
  align-items: center;
  border: 3px solid #ccc;
}
</style>
