<script setup>
defineProps({
  tasks: {
    type: Array,
    required: true,
  },
})

const emit = defineEmits(['toggle', 'delete'])
</script>

<template>
  <ul>
    <li v-for="task in tasks" :key="task.id" :class="{ done: task.completed }">
      <button class="task-text" @click="emit('toggle', task)">
        {{ task.text }}
      </button>
      <button class="delete-button" @click="emit('delete', task.id)">删除</button>
    </li>
  </ul>
</template>

<style scoped>
ul { padding: 0; list-style: none; }
li { display: flex; align-items: center; gap: 8px; margin: 10px 0; }
button { padding: 10px 16px; border: 0; border-radius: 8px; font: inherit; cursor: pointer; }
.task-text { flex: 1; text-align: left; color: #1e293b; background: #f8fafc; }
.delete-button { background: #e11d48; color: white; }
.done .task-text { color: #94a3b8; text-decoration: line-through; }
</style>