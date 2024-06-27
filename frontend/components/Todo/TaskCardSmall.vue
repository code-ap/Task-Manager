<template>
    <div class="mt-3">
        <div class="bg-white dark:bg-gray-500 p-2 rounded mb-2">
            <p class="text-gray-800 dark:text-gray-200">{{ task.title }}</p>
        </div>
    </div>
</template>


<script setup>
const props = defineProps({
    task: {
        type: Object,
        required: true
    }
});

// const supabase = useSupabaseClient()

// // Funktion, um Board-Details zu holen
// async function fetchTasks() {
//     const { data, error } = await supabase
//         .from('tasks')
//         .select('*')
//         .eq('list_id', list.id)

//     if (data) {
//         tasks.value = data
//     } else {
//         console.error('Fehler beim Abrufen der Board-Details:', error?.message)
//     }
// }

// onMounted(fetchTasks)
</script>