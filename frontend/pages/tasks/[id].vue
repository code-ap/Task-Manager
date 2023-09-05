<template>
  <TaskForm v-if="taskToEdit && Object.keys(taskToEdit).length" :mode="'edit'" :taskData="taskToEdit" />
</template>

<script setup>
const route = useRoute();
const supabase = useSupabaseClient();
const taskToEdit = ref({});

const fetchTask = async () => {
  const taskId = route.params.id;
  const { data, error } = await supabase.from('tasks').select('*').eq('id', taskId).single();
  if (data) {
    taskToEdit.value = data;
  } else {
    console.error("Error fetching task:", error);
  }
};

onMounted(fetchTask);
</script>
