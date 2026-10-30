
<script setup>
import { ref, onMounted } from 'vue'

const dogImage = ref('')
const loading = ref(false)
const error = ref('')

async function getDog() {
  loading.value = true
  error.value = ''

  try {
    const response = await fetch('https://dog.ceo/api/breeds/image/random')

    if (!response.ok) {
      throw new Error('Could not load dog picture')
    }

    const data = await response.json()
    dogImage.value = data.message
  } catch (err) {
    error.value = 'Something went wrong. Please try again!'
  } finally {
    loading.value = false
  }
}

onMounted(() => {
  getDog()
})
</script>

<template>
  <div class="dog-container">
    <h1>Random Dog Generator</h1>
    <p>Click the button to see a new dog!</p>

    <p v-if="loading">Loading dog picture...</p>

    <p v-if="error">{{ error }}</p>

    <img
      v-if="dogImage && !loading"
      :src="dogImage"
      alt="Random dog"
      class="dog-image"
    />

    <button @click="getDog" :disabled="loading">
      Get New Dog
    </button>
  </div>
</template>

<style scoped>
.dog-container {
  text-align: center;
  padding: 30px;
}

h1 {
  color: #a76dd6;
}

.dog-image {
  display: block;
  width: 350px;
  max-width: 100%;
  border-radius: 15px;
  margin: 20px auto;
}

button {
  background-color: #a76dd6;
  color: white;
  padding: 12px 24px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
}

button:hover {
  background-color: #8951b8;
}
</style>
