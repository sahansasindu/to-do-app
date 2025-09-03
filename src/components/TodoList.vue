<script setup>
import TodoItem from './TodoItem.vue'

const props = defineProps({
  tasks: Array
})

const emit = defineEmits(['toggle-task', 'delete-task'])
</script>

<template>
  <ul>
    <TodoItem
        v-for="task in tasks"
        :key="task.id"
        :task="task"
        @toggle-task="emit('toggle-task', task.id)"
        @delete-task="emit('delete-task', task.id)"
    />
  </ul>
</template>
