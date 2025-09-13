<template>
  <div :class="darkMode ? 'dark' : ''">
    <header class="flex justify-between items-center p-4 bg-gray-100 dark:bg-gray-900">
      <h1 class="text-xl font-bold text-gray-900 dark:text-white">Fenero</h1>
      <!-- Dark Mode Switch (Right Side) -->
      <button
        @click="toggleDarkMode"
        class="flex items-center gap-2 px-4 py-2 bg-white dark:bg-gray-800 text-gray-900 dark:text-white border rounded"
      >
        <span v-if="darkMode" class="flex items-center gap-2">
          <!-- Moon icon -->
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-5 w-5"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M21 12.79A9 9 0 1111.21 3 7 7 0 0021 12.79z"
            />
          </svg>
          Dark
        </span>
        <span v-else class="flex items-center gap-2">
          <!-- Sun icon -->
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-5 w-5"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M12 3v1m0 16v1m8.66-12.66l-.71.71M4.05 19.95l.71-.71M21 12h-1M4 12H3m16.66 4.95l-.71-.71M4.05 4.05l.71.71M12 8a4 4 0 100 8 4 4 0 000-8z"
            />
          </svg>
          Light
        </span>
      </button>
    </header>
    <!-- Page Content -->
    <main class="p-8 bg-gray-50 dark:bg-gray-800 min-h-screen text-gray-900 dark:text-gray-200">
      <RegistrationFormVue v-if="!registeredData" @registered="registeredData = $event" />
      <SuccessMessage v-else :data="registeredData" @reset="resetRegistration" />
    </main>
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue'
import RegistrationFormVue from './components/RegistrationForm.vue'
import SuccessMessage from './components/SuccessMessage.vue'
const darkMode = ref(false)
const registeredData = ref(null)
onMounted(() => {
  const savedMode = localStorage.getItem('darkMode')
  if (savedMode) darkMode.value = savedMode === 'true'
})
watch(darkMode, (newVal) => {
  localStorage.setItem('darkMode', newVal)
})
const toggleDarkMode = () => {
  darkMode.value = !darkMode.value
}
const resetRegistration = () => {
  registeredData.value = null // reset reactive state
  localStorage.removeItem('userRegistration') // clear saved form data
}
</script>

<style scoped></style>
