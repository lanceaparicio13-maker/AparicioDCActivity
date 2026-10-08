<script setup>
import { ref, onMounted } from 'vue'
import Resident from './components/Resident.vue'

const residents = ref([])

const newResident = ref({
  name: '',
  age: '',
  address: '',
  contact: ''
})

function addResident() {
  if (
    newResident.value.name.trim() === '' ||
    newResident.value.age === '' ||
    newResident.value.address.trim() === ''
  ) {
    return
  }

  residents.value.push({
    id: Date.now(),
    name: newResident.value.name,
    age: newResident.value.age,
    address: newResident.value.address,
    contact: newResident.value.contact
  })

  newResident.value = {
    name: '',
    age: '',
    address: '',
    contact: ''
  }

  saveResidents()
}

function deleteResident(id) {
  residents.value = residents.value.filter(
    resident => resident.id !== id
  )

  saveResidents()
}

function updateResident(updatedResident) {
  const index = residents.value.findIndex(
    resident => resident.id === updatedResident.id
  )

  if (index !== -1) {
    residents.value[index] = updatedResident
  }

  saveResidents()
}

function saveResidents() {
  localStorage.setItem(
    'residents',
    JSON.stringify(residents.value)
  )
}

onMounted(() => {
  const savedResidents = localStorage.getItem('residents')

  if (savedResidents) {
    residents.value = JSON.parse(savedResidents)
  }
})
</script>

<template>
  <div class="container">

    <h1>Barangay Information System</h1>

    <div class="add-resident">

      <input
        v-model="newResident.name"
        type="text"
        placeholder="Resident Name"
      />

      <input
        v-model="newResident.age"
        type="number"
        placeholder="Age"
      />

      <input
        v-model="newResident.address"
        type="text"
        placeholder="Address"
      />

      <input
        v-model="newResident.contact"
        type="text"
        placeholder="Contact Number"
      />

      <button @click="addResident">
        Add Resident
      </button>

    </div>

    <div class="resident-list">

      <Resident
        v-for="resident in residents"
        :key="resident.id"
        :resident="resident"
        @delete="deleteResident"
        @update="updateResident"
      />

    </div>

  </div>
</template>

<style>
.container {
  width: 600px;
  margin: 50px auto;
  font-family: Arial, sans-serif;
}

h1 {
  text-align: center;
}

.add-resident {
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin-bottom: 20px;
}

.add-resident input {
  padding: 10px;
}

button {
  padding: 10px 15px;
  cursor: pointer;
}

.resident-list {
  display: flex;
  flex-direction: column;
  gap: 10px;
}
</style>
