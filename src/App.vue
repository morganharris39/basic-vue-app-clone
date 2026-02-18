<template>
  <div class="flex justify-center items-center min-h-screen bg-gray-50 py-8">
    <div class="w-full max-w-md bg-pink-50 shadow-lg rounded-lg overflow-hidden">
      <!-- Header -->
      <div class="bg-pink-700 px-6 py-6 text-center">
        <h1 class="text-4xl font-bold text-white">Todo App</h1>
      </div>

      <!-- Main Content -->
      <div class="p-6 space-y-6">
        <!-- Add Todo Input -->
        <TodoInput @add-todo="addTodo" />

        <!-- Stats Section -->
        <TodoStats :remaining-count="remainingCount" />

        <!-- Todo List -->
        <div class="border-t border-pink-200 pt-4">
          <TodoList
            :todos="todos"
            @toggle-complete="toggleComplete"
            @delete-todo="deleteTodo"
            @start-edit="startEdit"
            @save-edit="saveEdit"
          />
        </div>

        <!-- Action Buttons -->
        <button
          v-if="todos.length > 0 && remainingCount < todos.length"
          @click="clearCompleted()"
          class="w-full bg-pink-700 text-white font-semibold py-4 px-4 rounded-md hover:bg-pink-800 transition-colors border-t border-pink-200 pt-4 mt-4"
        >
          Clear Completed
        </button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import TodoList from './components/TodoList.vue'
import TodoInput from './components/TodoInput.vue'
import TodoStats from './components/TodoStats.vue'

// Types
interface Todo {
  id: number
  text: string
  completed: boolean
  isEditing?: boolean
}

// State
const todos = ref<Todo[]>([
  { id: 1, text: 'Learn Vue.js', completed: false },
  { id: 2, text: 'Build todo app', completed: false },
  { id: 3, text: 'Master TypeScript', completed: true },
])

// Computed
const remainingCount = computed(() => {
  return todos.value.filter((todo) => !todo.completed).length
})

// Methods
function addTodo(text: string) {
  if (!text.trim()) return

  const newTodo: Todo = {
    id: Date.now(),
    text: text,
    completed: false,
  }
  todos.value.push(newTodo)
}

function deleteTodo(id: number) {
  todos.value = todos.value.filter((todo) => todo.id !== id)
}

function toggleComplete(id: number) {
  const todo = todos.value.find((todo) => todo.id === id)
  if (todo) {
    todo.completed = !todo.completed
  }
}

function startEdit(id: number) {
  todos.value = todos.value.map((todo) => ({
    ...todo,
    isEditing: todo.id === id ? true : false,
  }))
}

function saveEdit(id: number, newText: string) {
  todos.value = todos.value.map((todo) =>
    todo.id === id ? { ...todo, text: newText.trim(), isEditing: false } : todo,
  )
}

function clearCompleted() {
  todos.value = todos.value.filter((todo) => !todo.completed)
}
</script>
