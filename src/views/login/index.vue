<script setup lang="ts">
import { ref } from 'vue'

const username = ref('')
const password = ref('')
const loading = ref(false)
const error = ref('')

const handleSubmit = async (e: Event) => {
  e.preventDefault()
  error.value = ''
  loading.value = true
  try {
    const res = await fetch('http://127.0.0.1:8000/api/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        username: username.value,
        password: password.value
      })
    })
    const data = await res.json()
    if (!res.ok) {
      throw new Error(data.detail || '登录失败')
    }
    alert('登录成功')
    // 登录成功后的跳转或逻辑
  } catch (err: any) {
    error.value = err.message || '登录失败'
  } finally {
    loading.value = false
  }
}
</script>
<template>
  <div class="login-container">
    <h1>Login Page</h1>
    <form class="login-form" @submit="handleSubmit">
      <div class="form-group">
        <label for="username">Username:</label>
        <input
          type="text"
          id="username"
          name="username"
          v-model="username"
          :disabled="loading"
        />
      </div>
      <div class="form-group">
        <label for="password">Password:</label>
        <input
          type="password"
          id="password"
          name="password"
          v-model="password"
          :disabled="loading"
        />
      </div>
      <button type="submit" :disabled="loading">Login</button>
      <div v-if="error" style="color: red; margin-top: 8px;">{{ error }}</div>
    </form>
  </div>
</template>