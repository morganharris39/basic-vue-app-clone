<template>
  <ul v-if="props.todos.length > 0" class="space-y-2">
    <TodoItem
      v-for="todo in props.todos"
      :key="todo.id"
      :todo="todo"
      @toggle-complete="emit('toggle-complete', $event)"
      @delete-todo="emit('delete-todo', $event)"
      @start-edit="emit('start-edit', $event)"
      @save-edit="(id, text) => emit('save-edit', id, text)"
    />
  </ul>
  <div v-else class="text-center py-8 text-pink-400">
    <p class="text-lg">No todos yet. Add one to get started!</p>
  </div>
</template>

<script setup lang="ts">
import TodoItem from './TodoItem.vue'

// Types
interface Todo {
  id: number
  text: string
  completed: boolean
  isEditing?: boolean
}

// Props
interface Props {
  todos: Todo[]
}

const props = defineProps<Props>()

// Emits
const emit = defineEmits<{
  'toggle-complete': [id: number]
  'delete-todo': [id: number]
  'start-edit': [id: number]
  'save-edit': [id: number, text: string]
}>()
</script>
