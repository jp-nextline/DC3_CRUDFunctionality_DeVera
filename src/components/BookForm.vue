<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  editingBook: { type: Object, default: null },
})
const emit = defineEmits(['save', 'cancel'])

const title = ref('')
const author = ref('')
const status = ref('To Read')

function reset() {
  title.value = ''
  author.value = ''
  status.value = 'To Read'
}

watch(
  () => props.editingBook,
  (book) => {
    if (book) {
      title.value = book.title
      author.value = book.author
      status.value = book.status
    } else {
      reset()
    }
  },
  { immediate: true }
)

function submit() {
  if (!title.value.trim() || !author.value.trim()) return
  emit('save', {
    title: title.value.trim(),
    author: author.value.trim(),
    status: status.value,
  })
  reset()
}
</script>

<template>
  <form @submit.prevent="submit">
    <input v-model="title" placeholder="Book title" />
    <input v-model="author" placeholder="Author" />
    <select v-model="status">
      <option>To Read</option>
      <option>Reading</option>
      <option>Finished</option>
    </select>
    <button type="submit">{{ editingBook ? 'Update Book' : 'Add Book' }}</button>
    <button v-if="editingBook" type="but    ton" @click="emit('cancel')">Cancel</button>
  </form>
</template>