<template>
  <div class="wiki">
    <h1>Вики по кустарникам</h1>

    <div class="add-form">
      <input v-model="newPlant.name" placeholder="Название">
      <textarea v-model="newPlant.description" placeholder="Описание"></textarea>
      <button @click="addPlant">Добавить в базу</button>
    </div>

    <hr>

    <div v-for="plant in plants" :key="plant.id" class="card">
      <h3>{{ plant.name }}</h3>
      <p>{{ plant.description }}</p>
      <button @click="deletePlant(plant.id)">Удалить</button>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const plants = ref([])
const newPlant = ref({ name: '', description: '', is_hardy: true })

const API_URL = 'http://localhost:8000/api/plants/'


const fetchPlants = async () => {
  const res = await axios.get(API_URL)
  plants.value = res.data
}

const addPlant = async () => {
  await axios.post(API_URL, newPlant.value)
  newPlant.value = { name: '', description: '' }
  fetchPlants()
}

const deletePlant = async (id) => {
  await axios.delete(`${API_URL}${id}/`)
  fetchPlants()
}

onMounted(fetchPlants)
</script>

<style scoped>
.card { border: 1px solid #ccc; padding: 10px; margin: 10px 0; border-radius: 8px; }
.add-form { display: flex; flex-direction: column; gap: 10px; max-width: 300px; }
</style>