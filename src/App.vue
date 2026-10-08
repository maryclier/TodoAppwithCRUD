<script setup>
import { ref } from 'vue'
import ContactItem from './components/ContactItem.vue'

const contacts = ref([
  { id: 1, name: 'Ira Saren', phone: '09569913100', favorite: false },
  { id: 2, name: 'Erica Jaganas', phone: '09319883504', favorite: true },
])

const newName = ref('')
const newPhone = ref('')
let nextId = 3

function addContact() {
  if (newName.value.trim() === '' || newPhone.value.trim() === '') {
    alert('Please fill out both name and phone')
    return
  }
  contacts.value.push({
    id: nextId++,
    name: newName.value,
    phone: newPhone.value,
    favorite: false
  })
  newName.value = ''
  newPhone.value = ''
}

function updateContact(updatedContact) {
  const index = contacts.value.findIndex(c => c.id === updatedContact.id)
  if (index!== -1) {
    contacts.value[index] = updatedContact
  }
}

function deleteContact(id) {
  contacts.value = contacts.value.filter(c => c.id!== id)
}
</script>

<template>
  <div class="container">
    <h1>Contact List</h1>

    <div class="add-contact">
      <input v-model="newName" placeholder="Enter Name" @keyup.enter="addContact" />
      <input v-model="newPhone" placeholder="Enter Phone" @keyup.enter="addContact" />
      <button @click="addContact">Add Contact</button>
    </div>

    <div v-if="contacts.length === 0" class="empty">No contacts yet. Add one!</div>

    <ContactItem
      v-for="contact in contacts"
      :key="contact.id"
      :contact="contact"
      @update="updateContact"
      @delete="deleteContact"
    />
  </div>
</template>