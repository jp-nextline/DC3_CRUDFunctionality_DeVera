<script setup>
import { ref, onMounted } from 'vue'
import TaskAdd from './components/TaskAdd.vue'

const tasks = ref([])

const newTask = ref('')

const editingId = ref(null)
const editingTitle = ref('')

function addTask(task) {
  tasks.value.push({
    id: Date.now(),
    title: task,
    completed: false
  })

  saveTasks()
}

function deleteTask(id) {
  tasks.value = tasks.value.filter(task => task.id !== id)

  saveTasks()
}


function editTask(task) {
  editingId.value = task.id
  editingTitle.value = task.title
}

function updateTask(id) {
  if (editingTitle.value.trim() === '') {
    return
  }

  const task = tasks.value.find(task => task.id === id)

  if (task) {
    task.title = editingTitle.value
  }

  editingId.value = null
  editingTitle.value = ''

  saveTasks()
}

function cancelEdit() {
  editingId.value = null
  editingTitle.value = ''
}

function toggleTask(id) {
  const task = tasks.value.find(task => task.id === id)

  if (task) {
    task.completed = !task.completed
  }

  saveTasks()
}

function saveTasks() {
  localStorage.setItem(
    'tasks',
    JSON.stringify(tasks.value)
  )
}

onMounted(() => {
  const savedTasks = localStorage.getItem('tasks')

  if (savedTasks) {
    tasks.value = JSON.parse(savedTasks)
  }
})
</script>

<template>
  <div class="container">

    <h1>My Task List</h1>

    <TaskAdd @add="addTask" />

    <div class="task-list">

      <h2>Tasks</h2>

      <p v-if="tasks.length === 0" class="empty">
        No tasks available.
      </p>

      <div
        v-for="task in tasks"
        :key="task.id"
        class="task-card"
      >

        <div v-if="editingId !== task.id">

          <div class="task-content">

            <input
              type="checkbox"
              :checked="task.completed"
              @change="toggleTask(task.id)"
            />

            <span
              :class="{ completed: task.completed }"
            >
              {{ task.title }}
            </span>

          </div>

          <div class="buttons">

            <button
              class="edit"
              @click="editTask(task)"
            >
              Edit
            </button>

            <button
              class="delete"
              @click="deleteTask(task.id)"
            >
              Delete
            </button>

          </div>

        </div>

        <div v-else>

          <input
            v-model="editingTitle"
            class="edit-input"
            type="text"
            @keyup.enter="updateTask(task.id)"
          />

          <div class="buttons">

            <button
              class="save"
              @click="updateTask(task.id)"
            >
              Save
            </button>

            <button
              class="cancel"
              @click="cancelEdit"
            >
              Cancel
            </button>

          </div>

        </div>

      </div>

    </div>

  </div>
</template>

<style>
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  background: #f2f4f7;
  font-family: Arial, sans-serif;
}

.container {
  width: 500px;
  max-width: 90%;
  margin: 50px auto;
  background: white;
  padding: 30px;
  border-radius: 12px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
}

h1 {
  text-align: center;
  margin-bottom: 30px;
}

.task-list {
  margin-top: 30px;
}

.task-list h2 {
  margin-bottom: 15px;
}

.empty {
  text-align: center;
  color: #777;
}

.task-card {
  padding: 15px;
  margin-bottom: 12px;
  border: 1px solid #ddd;
  border-radius: 8px;
  background: #fafafa;
}

.task-content {
  display: flex;
  align-items: center;
  gap: 10px;
}

.task-content input {
  width: 18px;
  height: 18px;
}

.completed {
  text-decoration: line-through;
  color: #888;
}

.buttons {
  display: flex;
  gap: 8px;
  margin-top: 12px;
}

.buttons button {
  padding: 8px 14px;
  border: none;
  border-radius: 5px;
  color: white;
  cursor: pointer;
}

.edit {
  background: #3498db;
}

.delete {
  background: #e74c3c;
}

.save {
  background: #27ae60;
}

.cancel {
  background: #777;
}

.edit-input {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}
</style>