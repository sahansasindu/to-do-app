<script setup>
const props = defineProps({
  task: Object
})

const emit = defineEmits(['toggle-task', 'delete-task'])
</script>

<template>
  <li>
    <span
        :style="{ textDecoration: task.done ? 'line-through' : 'none' }"
        @click="emit('toggle-task')"
    >
      {{ task.text }}
    </span>
    <button @click="emit('delete-task')">❌</button>
  </li>
</template>
