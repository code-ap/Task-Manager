<template>
  <NuxtLink :to="`/tasks/${task.id}`">
    <div class="bg-white p-4 rounded-lg shadow-md mb-4">
      <h3 class="text-xl font-bold mb-2">{{ task.title }}</h3>
      <div class="text-sm text-gray-600 mb-2">
        <strong>Deadline:</strong> {{ task.deadlineDate }} at {{ task.deadlineTime }}
      </div>
      <p class="text-gray-700 truncate-2-lines">{{ task.description }}</p>
    </div>
  </NuxtLink>
</template>

<script>
export default {
  props: {
    task: {
      type: Object,
      required: true
    }
  }
}
</script>