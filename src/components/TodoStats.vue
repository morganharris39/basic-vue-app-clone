<template>
  <div class="bg-pink-100 rounded-md p-4 text-center">
    <p class="text-pink-900 font-semibold text-lg">
      {{ props.remainingCount }} {{ props.remainingCount === 1 ? 'todo' : 'todos' }} remaining
    </p>
  </div>
</template>

<script setup lang="ts">
// Props
interface Props {
  remainingCount: number
}

const props = defineProps<Props>()
</script>
