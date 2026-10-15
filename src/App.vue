
<script setup>
import { onMounted, ref } from 'vue'
import api from './services/api'

const users = ref([])
const isLoading = ref(true)
const errorMessage = ref('')

async function loadUsers() {
  isLoading.value = true
  errorMessage.value = ''

  try {
    const response = await api.get('/api/api/users')
    users.value = response.data.data
  } catch (error) {
    errorMessage.value = error.message
  } finally {
    isLoading.value = false
  }
}

onMounted(loadUsers)
</script>

<template>
  <main class="container">
    <h1>Daftar Pengguna</h1>
    <button @click="loadUsers">Muat Ulang</button>

    <p v-if="isLoading">Memuat data...</p>
    <p v-else-if="errorMessage" class="error">
      Gagal mengambil data: {{ errorMessage }}
    </p>
    <p v-else-if="users.length === 0">
      Data pengguna tidak ditemukan.
    </p>

    <ul v-else class="user-list">
      <li v-for="user in users" :key="user.id" class="user-card">
        <h2>{{ user.name }}</h2>
        <p>ID: {{ user.id }}</p>
        <p>Email: {{ user.email }}</p>
      </li>
    </ul>
  </main>
</template>

<style>
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  background: #f3f4f6;
  color: #1f2937;
  font-family: Arial, sans-serif;
}

.container {
  width: min(800px, calc(100% - 32px));
  margin: 40px auto;
}

button {
  padding: 10px 16px;
  border: none;
  border-radius: 6px;
  background: #2563eb;
  color: white;
  cursor: pointer;
}

.user-list {
  display: grid;
  gap: 16px;
  padding: 0;
  list-style: none;
}

.user-card {
  padding: 16px;
  border-radius: 8px;
  background: white;
  box-shadow: 0 2px 8px rgb(0 0 0 / 8%);
}

.user-card h2 {
  margin-top: 0;
  text-transform: capitalize;
}

.error {
  color: #b91c1c;
}
</style>
