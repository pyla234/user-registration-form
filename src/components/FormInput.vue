<template>
  <div class="mb-4 relative">
    <label class="block font-medium mb-1"
      >{{ label }}
      <span class="text-red-500">*</span>
    </label>
    <input
      :type="showPassword && type === 'password' ? 'text' : type"
      v-model="inputValue"
      :placeholder="placeholder"
      @input="$emit('input')"
      @blur="$emit('blur')"
      class="p-2 pr-10 border rounded w-full bg-white dark:bg-gray-700 text-gray-900 dark:text-gray-100 placeholder-gray-400 dark:placeholder-gray-300"
    />
    <!--  Toggle Button -->
    <button
      v-if="type === 'password'"
      type="button"
      class="absolute right-3 top-9 text-gray-500 dark:text-gray-300"
      @click="showPassword = !showPassword"
    >
      <!-- Eye Icon -->
      <svg
        v-if="showPassword"
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        stroke-width="1.5"
        stroke="currentColor"
        class="w-5 h-5"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          d="M2.036 12.322a1.012 1.012 0 010-.639C3.423 7.51 
             7.36 4.5 12 4.5c4.638 0 8.573 3.007 
             9.963 7.178.07.207.07.431 0 .639C20.577 
             16.49 16.64 19.5 12 19.5c-4.638 0-8.573-3.007-9.963-7.178z"
        />
        <path stroke-linecap="round" stroke-linejoin="round" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z" />
      </svg>

      <!-- Eye Slash Icon -->
      <svg
        v-else
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        stroke-width="1.5"
        stroke="currentColor"
        class="w-5 h-5"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          d="M3.98 8.223A10.477 10.477 0 001.934 
             12C3.226 16.338 7.244 19.5 12 
             19.5c.993 0 1.953-.138 
             2.863-.395M6.228 6.228A10.45 10.45 
             0 0112 4.5c4.756 0 8.773 
             3.162 10.065 7.5a10.523 
             10.523 0 01-4.293 5.774M6.228 
             6.228L3 3m3.228 3.228L21 21"
        />
      </svg>
    </button>
    <p v-if="error" class="text-red-500 text-sm mt-1">{{ error }}</p>
  </div>
</template>
<script setup>
import { computed, ref } from 'vue'

const props = defineProps({
  label: String,
  type: { type: String, default: 'text' },
  modelValue: [String, Number],
  error: String,
  placeholder: String,
})
const emit = defineEmits(['update:modelValue', 'input', 'blur'])
const showPassword = ref(false)
const inputValue = computed({
  get: () => props.modelValue,
  set: (val) => emit('update:modelValue', val),
})
</script>
