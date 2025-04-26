<script setup>
<script setup>
import { ref, computed, onMounted } from 'vue';

const tasks = ref([
  { name: "Belajar Vue", done: false },
  { name: "Mengerjakan tugas", done: true },
  { name: "Olahraga pagi", done: false },
  { name: "Membaca buku", done: true }
]);

const newTask = ref("");
const inputField = ref(null);

const isEditing = ref(false);
const editIndex = ref(null);
const showCompleted = ref(true);

const addTask = () => {
  if (newTask.value.trim() !== "") {
    tasks.value.push({ name: newTask.value.trim(), done: false });
    newTask.value = "";
  }
};

const deleteTask = (index) => {
  tasks.value.splice(index, 1);
  cancelEdit();
};

const editTask = (index) => {
  newTask.value = tasks.value[index].name;
  isEditing.value = true;
  editIndex.value = index;
  inputField.value.focus();
};

const updateTask = () => {
  if (editIndex.value !== null && newTask.value.trim() !== "") {
    tasks.value[editIndex.value].name = newTask.value.trim();
    cancelEdit();
  }
};

const cancelEdit = () => {
  newTask.value = "";
  isEditing.value = false;
  editIndex.value = null;
};

const filteredTasks = computed(() => {
  return showCompleted.value
    ? tasks.value
    : tasks.value.filter(task => !task.done);
});

onMounted(() => {
  inputField.value?.focus();
});

</script>

<template>
  <div id="app">
    <nav class="navbar">
      <div class="navbar-content">
        <h1>Andrino's Activity List</h1>
      </div>
    </nav> 
  </div>
  <main class="container">
      <div class="card">
        <h2>📋 DAFTAR KEGIATAN</h2>
      </div>
  </main>
</template>



<style scoped>

</style>
