<script setup>
import { ref } from 'vue'

const newTask = ref('')

const emit = defineEmits(['add'])

function addTask() {
  if (newTask.value.trim() === '') {
    return
  }

  emit('add', newTask.value)

  newTask.value = ''
}
</script>

<template>
  <div class="add-task">

    <input
      v-model="newTask"
      type="text"
      placeholder="Enter a task"
      @keyup.enter="addTask"
    />

    <button @click="addTask">
      Add Task
    </button>

  </div>
</template>

<style scoped>
.add-task {
  display: flex;
  gap: 10px;
}

.add-task input {
  flex: 1;
  padding: 10px;
  border: 1px solid #e4e4e4;
  border-radius: 5px;
}

.add-task button {
  padding: 10px 15px;
  border: none;
  border-radius: 5px;
  background: #28cc43;
  color: white;
  cursor: pointer;
}

.add-task button:hover {
  background: #0fac29;
}
</style>