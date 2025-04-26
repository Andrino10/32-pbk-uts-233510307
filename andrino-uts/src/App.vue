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
  <label class="toggle-show">
  <input type="checkbox" v-model="showCompleted" />
   Tampilkan yang sudah selesai
  </label>

  <ul class="task-list">
          <li v-for="(task, index) in filteredTasks" :key="index">
            <div class="task-content">
              <input type="checkbox" v-model="task.done" />
              <span :class="{ done: task.done }">{{ index + 1 }}. {{ task.name }}</span>
            </div>
            <div class="action-buttons">
              <button class="edit-btn" @click="editTask(index)">✏️</button>
              <button class="delete-btn" @click="deleteTask(index)">🗑️</button>
            </div>
          </li>
        </ul>
        <div class="input-group">
          <input ref="inputField" v-model="newTask" type="text" placeholder="Masukkan kegiatan baru" />
          <button v-if="isEditing" @click="updateTask">Update</button>
          <button v-else @click="addTask">Tambah</button>
          <button v-if="isEditing" class="cancel-btn" @click="cancelEdit">Batal</button>
        </div>
      </div>
      <footer class="footer">
      <p>© 2025 Andrino's App - Built with Vue 3</p>
    </footer>
    </main>
</template>



<style scoped>
html, body, #app {
  margin: 0;
  padding: 0;
  width: 100vw;
  height: 100vh;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: linear-gradient(to bottom right, #cfe9ff, #e6f4ff);
  overflow: hidden; 
}

.navbar {
  background-color: #0d6efd;
  padding: 15px;
  text-align: center;
}

.navbar-content h1 {
  color: #fff;
  margin: 0;
  font-size: 24px;
}

.container {
  width: 100%;
  height: calc(100vh - 120px); 
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 0 10px;
  box-sizing: border-box;
  overflow: hidden;
}


.card {
  background-color: #ffffff;
  border-radius: 20px;
  padding: 30px 20px;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.1);
  width: 100%;
  max-width: 420px;
  height: 100%;
  max-height: 600px;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  animation: fadeIn 0.8s ease;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}


h2 {
  color: #0d6efd;
  font-size: 26px;
  margin-bottom: 20px;
  text-align: center;
}

.toggle-show {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
  font-size: 15px;
  gap: 8px;
  color: #0d6efd;
}

.task-list {
  list-style: none;
  padding: 0;
  margin: 0;
  flex-grow: 1;
  overflow-y: auto;
}

.task-list li {
  padding: 10px 0;
  border-bottom: 1px solid #aad4ff;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.task-content {
  display: flex;
  align-items: center;
  gap: 10px;
  flex-wrap: wrap;
}

.done {
  text-decoration: line-through;
  color: #6c757d;
}

.action-buttons {
  display: flex;
  gap: 8px;
}

.edit-btn,
.delete-btn {
  background: none;
  border: none;
  cursor: pointer;
  font-size: 18px;
  color: #0d6efd;
  transition: color 0.2s;
}

.edit-btn:hover {
  color: #0b5ed7;
}

.delete-btn:hover {
  color: #dc3545;
}

.input-group {
  display: flex;
  gap: 10px;
  margin-top: 15px;
  flex-wrap: wrap;
}

input[type="text"] {
  flex: 1;
  padding: 10px;
  border: 1px solid #aad4ff;
  border-radius: 6px;
  font-size: 15px;
  background-color: #eaf6ff;
}

button {
  padding: 10px 12px;
  background-color: #0d6efd;
  color: white;
  border: none;
  border-radius: 6px;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #0b5ed7;
}

.cancel-btn {
  background-color: #dc3545;
}

.cancel-btn:hover {
  background-color: #bb2d3b;
}

.footer {
  background-color: #0d6efd;
  padding: 10px;
  text-align: center;
  color: white;
  font-size: 14px;
}
</style>
