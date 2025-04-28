<template>
  <div class="max-w-3xl mx-auto p-5">
    <h1 class="text-3xl font-bold text-center mb-8">Todo App</h1>

    <div class="flex-row items-center justify-center p-5">
      <h2 class="text-xl font-semibold mb-4">Add New Todo</h2>
      <input
        v-model="newTodo.title"
        placeholder="Title"
        class="w-full p-2 mb-3 border border-gray-300 rounded"
      />
      <textarea
        v-model="newTodo.description"
        placeholder="Description"
        class="w-full p-2 mb-3 border border-gray-300 rounded min-h-[100px]"
      ></textarea>
      <button @click="addTodo" class="bg-green-500 hover:bg-green-600 text-white px-4 py-2 rounded">
        Add Todo
      </button>
    </div>

    <div class=" p-5 rounded-lg">
      <h2 class="text-xl font-semibold mb-4">Your Todos</h2>
      <div v-if="todos.length === 0" class="text-center text-gray-500 py-5">
        No todos yet. Add one above!
      </div>
      <div v-for="(todo, index) in todos" :key="todo.id || index" class=" rounded p-4 mb-3 flex justify-between items-center">
        <div class="flex-1">
          <h3 class="font-semibold mb-1 text-white">{{ todo.title }}</h3>
          <p class="text-white">{{ todo.description }}</p>
        </div>
        <button @click="deleteTodo(todo)" class="bg-red-500 hover:bg-red-600 text-white px-3 py-2 rounded ml-4">
          Delete
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const todos = ref([])
const newTodo = ref({ title: '', description: '' })

const fetchTodos = async () => {
  const res = await fetch('/api/')
  todos.value = await res.json()
}

const addTodo = async () => {
  await fetch('/api/', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(newTodo.value)
  })
  newTodo.value = { title: '', description: '' }
  fetchTodos()
}

const deleteTodo = async (todo) => {
  await fetch('/api/' + todo.id, {
    method: 'DELETE',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(todo)
  })
  const lsit = []
  lsit.push()
  fetchTodos()
}

onMounted(fetchTodos)
</script>
