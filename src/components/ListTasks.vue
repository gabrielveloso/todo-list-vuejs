<script setup lang="ts">
import { Task } from "../types/task";
import { defineProps, TransitionGroup } from "vue";

const props = defineProps<{
  tasksFiltered: Task[];
}>();

const emit = defineEmits(["delete"]);

const deleteItem = (id: number) => {
  emit("delete", id);
};
</script>

<template>
  <div>
    <TransitionGroup name="list" tag="ul">
      <li class="item" v-for="task in tasksFiltered" :key="task.id">
        {{ task.name }} - {{ task.completed ? "Done" : "Pending" }}
        <div class="button-item">
          <button
            class="button-complete"
            v-if="!task.completed"
            @click="task.completed = true"
          >
            Mark as done
          </button>
          <i class="fas fa-trash-alt" @click="deleteItem(task.id)"></i>
        </div>
      </li>
    </TransitionGroup>
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

.button-item {
  display: flex;
  gap: 10px;
  align-items: center;

  i {
    cursor: pointer;
    color: darkred;
  }
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

.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}
.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}
</style>
