<script setup>
import { computed, ref, watch } from 'vue'
import TaskList from './components/TaskList.vue'

const studentName = '郭家祥'
const storageKey = 'frontend-internship-tasks'
const initialTasks = [
  { id: 1, text: '完善前端简历', completed: false },
  { id: 2, text: '投递实习岗位', completed: false },
  { id: 3, text: '完成一个 Vue 小练习', completed: false },
]

function loadTasks() {
  const savedTasks = localStorage.getItem(storageKey)

  return savedTasks ? JSON.parse(savedTasks) : initialTasks
}

const tasks = ref(loadTasks())
const newTask = ref('')
const currentFilter = ref('all')

watch(
  tasks,
  (currentTasks) => {
    localStorage.setItem(storageKey, JSON.stringify(currentTasks))
  },
  { deep: true },
)

const completedCount = computed(() => tasks.value.filter((task) => task.completed).length)
const totalCount = computed(() => tasks.value.length)
const progress = computed(() => {
  if (totalCount.value === 0) return 0

  return Math.round((completedCount.value / totalCount.value) * 100)
})

const filteredTasks = computed(() => {
  if (currentFilter.value === 'active') {
    return tasks.value.filter((task) => !task.completed)
  }

  if (currentFilter.value === 'completed') {
    return tasks.value.filter((task) => task.completed)
  }

  return tasks.value
})

function addTask() {
  const text = newTask.value.trim()

  if (!text) return

  tasks.value.push({ id: Date.now(), text, completed: false })
  newTask.value = ''
}

function toggleTask(task) {
  task.completed = !task.completed
}

function deleteTask(id) {
  tasks.value = tasks.value.filter((task) => task.id !== id)
}
</script>

<template>
  <main class="dashboard">
    <section class="card">
      <p class="eyebrow">FRONTEND INTERNSHIP</p>
      <h1>{{ studentName }}的前端实习看板</h1>
      <p>今天完成一小步，离实习机会更近一步。</p>
      <p class="progress">已完成 {{ completedCount }} / {{ totalCount }} 项（{{ progress }}%）</p>

      <h2>今日任务</h2>
      <form class="task-form" @submit.prevent="addTask">
        <input v-model="newTask" placeholder="输入一个学习任务" />
        <button type="submit">新增任务</button>
      </form>
      <div class="filters">
  <button :class="{ active: currentFilter === 'all' }" @click="currentFilter = 'all'">全部</button>
  <button :class="{ active: currentFilter === 'active' }" @click="currentFilter = 'active'">未完成</button>
  <button :class="{ active: currentFilter === 'completed' }" @click="currentFilter = 'completed'">已完成</button>
</div>

      <ul>
        <TaskList
  :tasks="filteredTasks"
  @toggle="toggleTask"
  @delete="deleteTask"
/>
      </ul>
    </section>
  </main>
</template>

<style scoped>
.dashboard { min-height: 100vh; padding: 48px 20px; background: #eef2ff; }
.card { max-width: 720px; margin: 0 auto; padding: 40px; border-radius: 20px; background: white; box-shadow: 0 16px 40px rgb(15 23 42 / 12%); }
.eyebrow, .progress { color: #2563eb; font-weight: 700; letter-spacing: 0.08em; }
h1 { margin-top: 0; color: #0f172a; }
h2 { margin-top: 32px; }
.task-form { display: flex; gap: 10px; }
input { flex: 1; min-width: 0; padding: 10px 12px; border: 1px solid #cbd5e1; border-radius: 8px; font: inherit; }
button { padding: 10px 16px; border: 0; border-radius: 8px; background: #2563eb; color: white; font: inherit; cursor: pointer; }
ul { padding: 0; list-style: none; }
li { display: flex; align-items: center; gap: 8px; margin: 10px 0; }
.task-text { flex: 1; text-align: left; color: #1e293b; background: #f8fafc; }
.delete-button { background: #e11d48; }
.done .task-text { color: #94a3b8; text-decoration: line-through; }
.filters { display: flex; gap: 8px; margin: 16px 0; }
.filters button { background: #e2e8f0; color: #475569; }
.filters button.active { background: #2563eb; color: white; }
</style>
