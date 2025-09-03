<script setup>
import { ref } from 'vue'

const newTask = ref('')
const emit = defineEmits(['add-task'])

const submitTask = () => {
  if (newTask.value.trim() !== '') {
    emit('add-task', newTask.value)
    newTask.value = '' // clear input
  }
}
</script>

<template>
  <div class="my-2">
    <input
        v-model="newTask"
        placeholder="Enter new task"
    />
    <button @click="submitTask">Add</button>
  </div>
</template>
