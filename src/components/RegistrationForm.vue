<template>
  <div>
    <form
      @submit.prevent="handleSubmit"
      class="max-w-lg mx-auto bg-white dark:bg-gray-700 text-gray-900 dark:text-gray-100 p-6 rounded-xl shadow-md"
    >
      <h2 class="text-2xl font-bold mb-4">User Registration</h2>
      <!-- Full Name -->
      <FormInput
        label="Full Name"
        placeholder="Enter Full Name"
        type="text"
        v-model="form.fullName"
        :error="touched.fullName ? errors.fullName : ''"
        @input="validate()"
        @blur="handleBlur('fullName')"
      />
      <!-- Email -->
      <FormInput
        label="Email"
        placeholder="Enter Email id"
        type="email"
        v-model="form.email"
        :error="touched.email ? errors.email : ''"
        @input="validate()"
        @blur="handleBlur('email')"
      />
      <!-- Phone Number -->
      <FormInput
        label="Phone Number"
        placeholder="Enter Phone Number"
        type="tel"
        v-model="form.phone"
        :error="touched.phone ? errors.phone : ''"
        @input="validate()"
        @blur="handleBlur('phone')"
      />
      <!-- Password -->
      <FormInput
        label="Password"
        placeholder="Enter Password"
        type="password"
        v-model="form.password"
        :error="touched.password ? errors.password : ''"
        @input="validate()"
        @blur="handleBlur('password')"
      />
      <!-- Password Strength Indicator -->
      <div v-if="form.password" class="mt-1">
        <div class="h-2 w-full bg-gray-200 rounded">
          <div
            class="h-2 rounded transition-all"
            :class="{
              'w-1/5 bg-red-500': passwordStrength === 1,
              'w-2/5 bg-orange-500': passwordStrength === 2,
              'w-3/5 bg-yellow-500': passwordStrength === 3,
              'w-4/5 bg-blue-500': passwordStrength === 4,
              'w-full bg-green-500': passwordStrength === 5,
            }"
          ></div>
        </div>
        <p
          class="text-sm mt-1"
          :class="{
            'text-red-500': passwordStrength <= 2,
            'text-yellow-600': passwordStrength === 3,
            'text-green-600': passwordStrength >= 4,
          }"
        >
          {{
            passwordStrength <= 2
              ? 'Weak'
              : passwordStrength === 3
                ? 'Medium'
                : passwordStrength === 4
                  ? 'Strong'
                  : 'Very Strong'
          }}
        </p>
      </div>
      <!-- Confirm Password -->
      <FormInput
        label="Confirm Password"
        placeholder="Enter Confirm Password"
        type="password"
        v-model="form.confirmPassword"
        :error="touched.confirmPassword ? errors.confirmPassword : ''"
        @input="validate()"
        @blur="handleBlur('confirmPassword')"
      />
      <!-- Gender -->
      <div class="mb-4">
        <label class="block font-medium mb-1">Gender<span class="text-red-500">*</span></label>
        <div class="flex gap-4" @change="handleGenderChange">
          <label><input type="radio" value="Male" v-model="form.gender" /> Male</label>
          <label><input type="radio" value="Female" v-model="form.gender" /> Female</label>
          <label><input type="radio" value="Other" v-model="form.gender" /> Other</label>
        </div>
        <p v-if="errors.gender && touched.gender" class="text-red-500 text-sm">
          {{ errors.gender }}
        </p>
      </div>
      <!-- DOB -->
      <div class="mb-4">
        <label class="block font-medium mb-1"
          >Date of Birth<span class="text-red-500">*</span></label
        >
        <input
          type="date"
          class="w-full px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent text-gray-700"
          v-model="form.dob"
          @input="validate()"
          @blur="handleBlur('dob')"
        />
        <p v-if="errors.dob && touched.dob" class="text-red-500 text-sm">{{ errors.dob }}</p>
      </div>
      <!-- Country -->
      <div class="mb-4">
        <label class="block font-medium mb-1">Country<span class="text-red-500">*</span></label>
        <select
          v-model="form.country"
          @input="validate()"
          @blur="handleBlur('country')"
          class="w-full p-2 border rounded-lg bg-white dark:bg-gray-700 text-gray-900 dark:text-gray-100 placeholder-gray-400 dark:placeholder-gray-300"
        >
          <option value="">Select Country</option>
          <option>India</option>
          <option>USA</option>
          <option>UK</option>
          <option>Canada</option>
          <option>Australia</option>
        </select>
        <p v-if="errors.country && touched.country" class="text-red-500 text-sm">
          {{ errors.country }}
        </p>
      </div>
      <!-- Profile Image Upload -->
      <div class="mb-4">
        <label class="block font-medium mb-1">Profile Image</label>
        <input
          type="file"
          accept="image/*"
          ref="fileInput"
          @change="handleImageUpload"
          class="block w-full text-sm text-gray-900 border border-gray-300 rounded-lg cursor-pointer bg-white dark:bg-gray-700 dark:text-gray-100"
        />
        <div v-if="form.profilePreview" class="mt-2 flex items-center gap-4">
          <img
            :src="form.profilePreview"
            alt="Preview"
            class="h-20 w-20 rounded-full object-cover"
          />
          <button
            type="button"
            @click="removeImage"
            class="bg-red-500 text-white px-3 py-1 rounded-lg text-sm hover:bg-red-600"
          >
            Remove
          </button>
        </div>
      </div>
      <!-- Terms -->
      <div class="mb-4">
        <label>
          <input
            type="checkbox"
            v-model="form.terms"
            @input="validate()"
            @change="handleBlur('terms')"
          />
          I agree to the terms and conditions
        </label>
        <p v-if="errors.terms && touched.terms" class="text-red-500 text-sm">{{ errors.terms }}</p>
      </div>
      <!-- Buttons -->
      <div class="flex gap-4">
        <button
          type="submit"
          :disabled="!isFormValid || loading"
          class="bg-blue-500 text-white font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 flex disabled:opacity-50"
        >
          Register &nbsp;
          <svg
            v-if="loading"
            class="animate-spin h-5 w-5 text-white"
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
          >
            <circle
              class="opacity-25"
              cx="12"
              cy="12"
              r="10"
              stroke="currentColor"
              stroke-width="4"
            ></circle>
            <path
              class="opacity-75"
              fill="currentColor"
              d="M4 12a8 8 0 018-8v4a4 4 0 00-4 4H4z"
            ></path>
          </svg>
        </button>
        <button
          @click="resetForm"
          type="button"
          class="text-gray-900 bg-white border border-gray-300 focus:outline-none hover:bg-gray-100 focus:ring-4 focus:ring-gray-100 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:bg-gray-800 dark:text-white dark:border-gray-600 dark:hover:bg-gray-700 dark:hover:border-gray-600 dark:focus:ring-gray-700"
        >
          Clear
        </button>
      </div>
    </form>
  </div>
</template>
<script setup>
import FormInput from './FormInput.vue'
import { reactive, computed, ref, onMounted } from 'vue'
const emit = defineEmits(['registered'])

const form = reactive({
  fullName: '',
  email: '',
  phone: '',
  password: '',
  confirmPassword: '',
  gender: '',
  dob: '',
  country: '',
  terms: false,
  profileImage: null,
  profilePreview: '',
})

const errors = reactive({
  fullName: '',
  email: '',
  phone: '',
  password: '',
  confirmPassword: '',
  gender: '',
  dob: '',
  country: '',
  terms: '',
})

const touched = reactive({
  fullName: false,
  email: false,
  phone: false,
  password: false,
  confirmPassword: false,
  gender: false,
  dob: false,
  country: false,
  terms: false,
})

const fileInput = ref(null)
const loading = ref(false)

// checking password strength
const getPasswordStrength = (password) => {
  let strength = 0
  if (password.length >= 8) strength++ // length check
  if (/[A-Z]/.test(password)) strength++ // uppercase
  if (/[a-z]/.test(password)) strength++ // lowercase
  if (/\d/.test(password)) strength++ // number
  if (/[@$!%*?&]/.test(password)) strength++ // special char
  return strength
}

const passwordStrength = computed(() => getPasswordStrength(form.password))

// Errors Validation
const validate = () => {
  // Full Name
  if (!form.fullName) {
    errors.fullName = 'Full name is required.'
  } else if (form.fullName.length < 3) {
    errors.fullName = 'Full name must be at least 3 characters'
  } else {
    errors.fullName = ''
  }

  // Email
  if (!form.email) {
    errors.email = 'Email is required.'
  } else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(form.email)) {
    errors.email = 'Invalid email'
  } else {
    errors.email = ''
  }

  // Phone
  if (!form.phone) {
    errors.phone = 'Phone Number is required.'
  } else if (!/^\d{10}$/.test(form.phone)) {
    errors.phone = 'Phone must be 10 digits'
  } else {
    errors.phone = ''
  }

  // Password
  if (!form.password) {
    errors.password = 'Password is required.'
  } else if (!/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d).{8,}$/.test(form.password)) {
    errors.password = 'Password must be 8+ chars, include upper, lower, and number'
  } else {
    errors.password = ''
  }

  // Confirm Password
  if (!form.confirmPassword) {
    errors.confirmPassword = 'Confirm Password is required.'
  } else if (form.confirmPassword !== form.password) {
    errors.confirmPassword = 'Passwords do not match'
  } else {
    errors.confirmPassword = ''
  }

  // Gender
  errors.gender = !form.gender ? 'Gender is required' : ''

  // Date of Birth
  dobValidation()

  // Country
  errors.country = !form.country ? 'Select a country' : ''

  // Terms
  errors.terms = !form.terms ? 'You must agree to terms' : ''
}

// DOB Validation
const dobValidation = () => {
  if (form.dob) {
    const today = new Date()
    const birthDate = new Date(form.dob)
    let age = today.getFullYear() - birthDate.getFullYear()
    const monthDiff = today.getMonth() - birthDate.getMonth()
    if (monthDiff < 0 || (monthDiff === 0 && today.getDate() < birthDate.getDate())) {
      age--
    }
    errors.dob = age < 18 ? 'You must be 18+' : ''
  } else {
    errors.dob = 'Date of birth is required'
  }
}

const handleBlur = (field) => {
  touched[field] = true
  validate()
}

const handleGenderChange = () => {
  touched.gender = true
  validate()
}
// Image upload
const handleImageUpload = (event) => {
  const file = event.target.files[0]
  if (file) {
    form.profileImage = file

    const reader = new FileReader()
    reader.onload = (e) => {
      form.profilePreview = e.target.result // Base64 string
    }
    reader.readAsDataURL(file)
  }
}
// Remove image
const removeImage = () => {
  form.profileImage = null
  form.profilePreview = ''
  if (fileInput.value) {
    fileInput.value.value = ''
  }
}

const isFormValid = computed(() => {
  return Object.values(errors).every((err) => err === '')
})

// submited form data
const handleSubmit = async () => {
  validate()
  if (!isFormValid.value) return
  loading.value = true
  try {
    await new Promise((resolve) => setTimeout(resolve, 1000))
    localStorage.setItem('userRegistration', JSON.stringify(form))
    console.log('Form Data:', JSON.stringify(form, null, 2))
    emit('registered', { ...form }) // send data to parent
    Object.keys(form).forEach((key) => (form[key] = key === 'terms' ? false : ''))
  } catch (err) {
    console.error(err)
  } finally {
    loading.value = false // stop spinner
  }
}

const resetForm = () => {
  Object.keys(form).forEach((key) => (form[key] = key === 'terms' ? false : ''))
  localStorage.removeItem('userRegistration') // clear saved form data;
}
// On Page Load
const onPageLoad = () => {
  const savedData = localStorage.getItem('userRegistration')
  if (savedData) {
    const parsed = JSON.parse(savedData)
    Object.keys(form).forEach((key) => {
      if (parsed[key] !== undefined) form[key] = parsed[key]
    })
  }
}
onMounted(() => {
  onPageLoad()
})
</script>
