<template>
  <li
    class="bg-white border border-pink-200 rounded-md p-4 flex justify-between items-center gap-4 hover:shadow-md transition-shadow"
  >
    <div class="flex items-center gap-3 flex-1 min-w-0">
      <input
        type="checkbox"
        :checked="props.todo.completed"
        @change="emit('toggle-complete', props.todo.id)"
        class="w-5 h-5 text-pink-600 rounded cursor-pointer"
      />

      <!-- Edit Mode -->
      <input
        v-if="props.todo.isEditing"
        v-click-outside="unselectItem"
        :value="props.todo.text"
        @keyup.enter="handleSaveEdit"
        @blur="handleSaveEdit"
        @input="editText = ($event.target as HTMLInputElement).value"
        class="flex-1 px-3 py-1 border border-pink-400 rounded-md focus:outline-none focus:ring-2 focus:ring-pink-500 focus:border-transparent"
        ref="editInputRef"
      />

      <!-- Display Mode -->
      <span
        v-else
        :class="{
          'line-through text-gray-400': props.todo.completed,
          'text-gray-800': !props.todo.completed,
        }"
        @click="emit('start-edit', props.todo.id)"
        class="flex-1 cursor-pointer hover:text-pink-600 transition-colors truncate"
      >
        {{ props.todo.text }}
      </span>
    </div>

    <button
      @click="emit('delete-todo', props.todo.id)"
      class="bg-red-600 text-white px-3 py-1 rounded-md hover:bg-red-700 transition-colors font-medium whitespace-nowrap text-sm"
    >
      Delete
    </button>
  </li>
</template>

<script setup lang="ts">
import { ref, watch, nextTick } from 'vue'

// Types
interface Todo {
  id: number
  text: string
  completed: boolean
  isEditing?: boolean
}

// Props
interface Props {
  todo: Todo
}

const props = defineProps<Props>()

// Emits
const emit = defineEmits<{
  'toggle-complete': [id: number]
  'delete-todo': [id: number]
  'start-edit': [id: number]
  'save-edit': [id: number, text: string]
}>()

// Local state for editing
const editText = ref(props.todo.text)
const editInputRef = ref<HTMLInputElement | null>(null)

watch(
  () => props.todo.isEditing,
  (isEditing) => {
    if (isEditing) {
      nextTick(() => {
        editInputRef.value?.focus()
        editInputRef.value?.select()
      })
    }
  },
)

// Methods
function handleSaveEdit() {
  emit('save-edit', props.todo.id, editText.value)
}

function unselectItem() {
  emit('save-edit', props.todo.id, editText.value)
}
</script>
