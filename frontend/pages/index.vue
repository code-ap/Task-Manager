<template>
  <div>
    <!-- Tasks List -->
    <div class="p-6 flex space-x-6">
        <TaskCard v-for="task in tasks" :key="task.id" :task="task" />
    </div>
    <!-- Add New Task Button -->
    <button @click="navigateToCreate" class="p-2 bg-green-500 text-white rounded-md">Add New Task</button>
  </div>
</template>



<script setup lang="ts">
const supabase = useSupabaseClient();
const tasks = ref([]);

onMounted(async () => {
  const { data, error } = await supabase.from('tasks').select('*');
  if (error) {
    console.error(error);
  } else {
    tasks.value = data;
  }
});

const navigateToCreate = () => {
  navigateTo('/tasks/create');
};
</script>
