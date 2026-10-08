<script setup>
import BookForm from './components/BookForm.vue'
import { ref, watch } from 'vue'

const books = ref(JSON.parse(localStorage.getItem('books') || '[]'))
const editingBook = ref(null)

watch(books, (value) => {
  localStorage.setItem('books', JSON.stringify(value))
}, { deep: true })

function startEdit(book) {
  editingBook.value = book
}

function cancelEdit() {
  editingBook.value = null
}

function saveBook(data) {
  if (editingBook.value) {
    books.value = books.value.map(book =>
      book.id === editingBook.value.id ? { ...book, ...data } : book
    )
    editingBook.value = null
  } else {
    books.value.push({ id: Date.now(), ...data })
  }
}

function deleteBook(id) {
  if (!confirm('Delete this book?')) return
  books.value = books.value.filter(book => book.id !== id)
}

</script>

<template>
  <main>
    <h1>My Book Library</h1>

    <BookForm :editing-book="editingBook" @save="saveBook" @cancel="cancelEdit" />

    <p v-if="books.length === 0">No books yet. Add one above.</p>

    <ul>
      <li v-for="book in books" :key="book.id" :class="{ finished: book.status === 'Finished' }">

        <span class="info">
          <strong>{{ book.title }}</strong> by {{ book.author }} — {{ book.status }}
        </span>
        
        <button @click="startEdit(book)">Edit</button>
        <button @click="deleteBook(book.id)">Delete</button>
      </li>
    </ul>
  </main>
</template>