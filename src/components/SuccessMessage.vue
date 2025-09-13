<template>
  <div class="bg-white dark:bg-gray-700 shadow-md rounded-xl p-6 max-w-md mx-auto mt-6">
    <h2 class="text-lg sm:text-xl md:text-2xl font-bold text-green-600 mb-4">
      Registration Successful!
    </h2>

    <!-- Profile Image -->
    <div v-if="data.profilePreview" class="flex justify-center mb-4">
      <img
        :src="data.profilePreview"
        alt="Profile"
        class="h-24 w-24 rounded-full object-cover border-2 border-gray-300 dark:border-gray-600"
      />
    </div>
    <!-- User Details -->
    <ul class="text-gray-900 dark:text-gray-100 space-y-2 text-center sm:text-left">
      <li><strong>Name:</strong> {{ data.fullName }}</li>
      <li><strong>Email:</strong> {{ data.email }}</li>
      <li><strong>Phone:</strong> {{ data.phone }}</li>
      <li><strong>Gender:</strong> {{ data.gender }}</li>
      <li><strong>DOB:</strong> {{ formatDate(data.dob) }}</li>
      <li><strong>Country:</strong> {{ data.country }}</li>
    </ul>
    <button
      @click="$emit('reset')"
      class="mt-4 w-full bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600"
    >
      Register Another User
    </button>
  </div>
</template>

<script setup>
const props = defineProps({
  data: { type: Object, required: true },
})
const formatDate = (dateStr) => {
  if (!dateStr) return ''
  const date = new Date(dateStr)
  const day = String(date.getDate()).padStart(2, '0')
  const month = String(date.getMonth() + 1).padStart(2, '0')
  const year = date.getFullYear()
  return `${day}-${month}-${year}`
}
</script>
