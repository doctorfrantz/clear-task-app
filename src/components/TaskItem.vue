<template>
  <div class="card">
    <div
      class="todo-item animate__animated animate__fadeInRight"
      v-for="(task, index) in tasks"
      :key="index">
      <div class="todo-card">
        
        <div class="todo-buttons">
          <div
          :class="
              task.is_complete ? 'todo-change-state' : 'todo-change-state-not'
            "
            @click="toggleReminderTask(task.id, index)"
            ><div :class="task.is_complete ? 'todo-image' : 'todo-image-not'">
              <i v-if="task.is_complete" class="task-completed"><img class="task-img-completed" src="../assets/img/CheckBoxCompleted.png" alt="Task Completed!">Completed</i>
              <i v-if="!task.is_complete" class="task-not-completed"><img class="task-img-not-completed" src="../assets/img/CheckBox.png" alt="Task to complete">Mark as done</i>
            </div></div>
            
            <div class="todo-change-name">
              <i
              class="change"
              @click="changeNameActiveValue(task)"
              ><img class="img-icon" src="../assets/img/Edit.png" alt="Edit Task"></i>
            </div>
            
            <div class="todo-delete">
              <i class="change" @click="deleteTask(task.id)"><img class="img-icon" src="../assets/img/Delete.png" alt="Delete Task"></i>
            </div>
          </div>
          
          <div class="changeName" v-if="changeNameActive && idRef === task.id">
            <div class="add-task-form">
              <div>
                <input
                class="input-field-edit"
                type="text"
                placeholder="Edit title"
                v-model="name"
                />
              </div>
              
              <div>
                <input
                class="input-field-edit"
                type="text"
                placeholder="Edit description"
                v-model="description"
                />
              </div>
              
              <button @click="changeNameTask(task.id, index)" class="button-change-name">
                Change
              </button>
            </div>
          </div>
          <div class="todo-title">
            <h3 :class="task.is_complete ? 'done' : ''">{{ task.title }}</h3>
          </div>
  
          <div class="todo-description">
            <p :class="task.is_complete ? 'done' : ''">{{ task.description }}</p>
          </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const emit = defineEmits(["toggle-reminder", "delete-task", "change-name"]);

const props = defineProps({
  tasks: Array,
});

const name = ref("");
const description = ref("");

const changeNameActive = ref(false);
const idRef = ref(null);

const toggleReminderTask = (id, index) => {
  const taskToToggle = props.tasks.map((task) =>
    task.id === id ? { ...task, is_complete: !task.is_complete } : task
  );

  emit("toggle-reminder", taskToToggle[index]);
};
const deleteTask = (id) => {
  const taskToDelete = props.tasks.filter((task) => task.id === id);
  emit("delete-task", taskToDelete[0]);
};

const changeNameActiveValue = (task) => {
  idRef.value = task.id;
  changeNameActive.value = !changeNameActive.value;
  name.value = task.title;
  description.value = task.description;
};

const changeNameTask = (id, index) => {
  const taskToEdit = props.tasks.map((task) =>
    task.id === id
      ? { ...task, title: name.value, description: description.value }
      : task
  );
  emit("change-name", taskToEdit[index]);

  name.value = "";
  description.value = "";
  changeNameActive.value = false;
};
</script>

<style scoped>
</style>