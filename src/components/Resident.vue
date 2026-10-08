<script setup>
import { ref } from 'vue'

const props = defineProps({
resident: Object
})

const emit = defineEmits(['delete', 'update'])

const editing = ref(false)

const editResident = ref({
id: props.resident.id,
name: props.resident.name,
age: props.resident.age,
address: props.resident.address,
contact: props.resident.contact
})

function startEdit() {
editResident.value = {
    id: props.resident.id,
    name: props.resident.name,
    age: props.resident.age,
    address: props.resident.address,
    contact: props.resident.contact
}

editing.value = true
}

function saveEdit() {
if (
    editResident.value.name.trim() === '' ||
    editResident.value.age === '' ||
    editResident.value.address.trim() === ''
) {
    return
}

emit('update', editResident.value)

editing.value = false
}
</script>

<template>

<div v-if="editing" class="resident-item">

    <input
    v-model="editResident.name"
    type="text"
    placeholder="Resident Name"
    />

    <input
    v-model="editResident.age"
    type="number"
    placeholder="Age"
    />

    <input
    v-model="editResident.address"
    type="text"
    placeholder="Address"
    />

    <input
    v-model="editResident.contact"
    type="text"
    placeholder="Contact Number"
    />

    <button @click="saveEdit">
    Save
    </button>

    <button @click="editing = false">
    Cancel
    </button>

</div>

<div v-else class="resident-item">

    <div>
    <h3>{{ resident.name }}</h3>
    <p>Age: {{ resident.age }}</p>
    <p>Address: {{ resident.address }}</p>
    <p>
        Contact:
        {{ resident.contact || 'No contact number' }}
    </p>
    </div>

    <div class="actions">

    <button @click="startEdit">
        Edit
    </button>

    <button @click="$emit('delete', resident.id)">
        Delete
    </button>

    </div>

</div>

</template>

<style>
.resident-item {
border: 1px solid #ccc;
padding: 15px;
display: flex;
justify-content: space-between;
gap: 15px;
}

.resident-item h3 {
margin: 0 0 10px;
}

.resident-item p {
margin: 5px 0;
}

.resident-item input {
display: block;
width: 100%;
padding: 8px;
margin-bottom: 8px;
box-sizing: border-box;
}

.actions {
display: flex;
gap: 5px;
align-items: center;
}

button {
padding: 8px 12px;
cursor: pointer;
}
</style>
