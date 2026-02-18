<template>
  <div class="flex gap-3">
    <input
      v-model="newTodoText"
      @keyup.enter="handleAddTodo"
      autocomplete="off"
      type="text"
      placeholder="Add a new todo..."
      class="flex-1 px-4 py-2 border border-pink-200 rounded-md focus:outline-none focus:ring-2 focus:ring-pink-500 focus:border-transparent bg-white"
    />
    <button
      @click="handleAddTodo"
      class="text-white font-semibold bg-pink-700 px-6 py-2 rounded-md hover:bg-pink-800 transition-colors whitespace-nowrap"
    >
      Add
    </button>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

// Local state
const newTodoText = ref('')

// Emits
const emit = defineEmits<{
  'add-todo': [text: string]
}>()

// Methods
function handleAddTodo() {
  if (!newTodoText.value.trim()) return

  emit('add-todo', newTodoText.value)
  newTodoText.value = '' // Clear input after emitting
}
</script>
