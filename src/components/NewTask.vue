<template>
  <div class="add-task-section">
    <div class="add-task-title">
      <h1 class="title">Add a Task</h1>
    </div>

    <div class="add-task-description">
      <p class="add-task-quote">
        “The humblest tasks get beautified if loving hands do them.”
      </p>
      <p class="add-task-quote-author">
        ― Louisa May Alcott, Little Women
      </p>
    </div>

    <div v-if="showErrorMessage">
      <p class="error-text">{{ errorMessage }}</p>
    </div>

    <div class="add-task-form">
      <div class="input-field-task">
        <input
          class="input-field-input"
          type="text"
          placeholder="Add a Task Title"
          v-model="name"
        />
      </div>

      <div class="input-field-task">
        <input
          class="input-field-input"
          type="text"
          placeholder="Add a Task Description"
          v-model="description"
        />
      </div>
      <button @click="errorFunction" class="add-button">Add</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { useTaskStore } from "../stores/task";

const setTask = useTaskStore();


const name = ref("");
const description = ref("");


const showErrorMessage = ref(false);
const errorMessage = ref(null);

const emit = defineEmits(["add-task"]);


const errorFunction = () => {
  if (name.value.length === 0 || description.value.length === 0) {
    showErrorMessage.value = true;
    errorMessage.value = "The task title or description is empty";
    setTimeout(() => {
      showErrorMessage.value = false;
    }, 5000);
  } else {
    const newTask = {
      name: name.value,
      description: description.value,
    };

    emit("add-task", newTask);
    name.value = "";
    description.value = "";
  }
};
</script>

<style scoped>
</style>
