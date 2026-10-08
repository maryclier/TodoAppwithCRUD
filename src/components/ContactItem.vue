<script setup>
import { ref } from 'vue'

const props = defineProps({ contact: Object })
const emit = defineEmits(['delete', 'update'])

const isEditing = ref(false)
const editedName = ref(props.contact.name)
const editedPhone = ref(props.contact.phone)

function updateContact() {
  if (editedName.value.trim() === '' || editedPhone.value.trim() === '') return
  emit('update', {
 ...props.contact,
    name: editedName.value,
    phone: editedPhone.value
  })
  isEditing.value = false
}

function toggleFavorite() {
  emit('update', {...props.contact, favorite:!props.contact.favorite })
}

function deleteContact() {
  emit('delete', props.contact.id)
}

function startEditing() {
  editedName.value = props.contact.name
  editedPhone.value = props.contact.phone
  isEditing.value = true
}
</script>

<template>
  <div class="contact-item">
    <!-- VIEW MODE -->
    <div v-if="!isEditing" class="view-mode">
      <button @click="toggleFavorite" class="star-btn">
        {{ contact.favorite? 'Fav' : 'Not Fav' }}
      </button>
      <span :class="{ favorite: contact.favorite }">
        {{ contact.name }} - {{ contact.phone }}
      </span>
      <button @click="startEditing">Edit</button>
      <button @click="deleteContact">Delete</button>
    </div>

    <!-- EDIT MODE -->
    <div v-else class="edit-mode">
      <input v-model="editedName" placeholder="Name" />
      <input v-model="editedPhone" placeholder="Phone" />
      <button @click="updateContact">Save</button>
      <button @click="isEditing = false">Cancel</button>
    </div>
  </div>
</template>