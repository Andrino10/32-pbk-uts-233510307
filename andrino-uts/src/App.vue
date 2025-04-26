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
</script>

<template>

</template>

<style scoped>

</style>
