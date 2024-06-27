<template>
    <!-- Einzelne Liste -->
    <div class="flex-none w-64 bg-gray-100 dark:bg-gray-600 p-2 rounded">
        <h2 class="m-1 font-semibold text-center text-gray-900 dark:text-white">
            <ListItemComponent :item="list" @click="goToListView" @delete="deleteList" />
        </h2>
        <!-- Container für Tasks mit festgelegter maximaler Höhe und vertikalem Scroll -->
        <div class="max-h-[calc(100vh-12rem)] overflow-y-auto">
            <!-- Tasks -->
            <!-- <TodoTaskCardSmall v-for="task in tasks" :task="task"/> -->
            <ListItemComponent v-for="task in tasks" :item="task" @click="goToTaskView" @delete="deleteTask" />
        </div>
        <!-- Neuer Task -->
        <CreateItemComponent name="Neuer Task" placeholder="Task-Titel" @create="createTask" />
    </div>
</template>

<script setup>
const { list } = defineProps({
    list: {
        type: Object,
        required: true
    }
});

const supabase = useSupabaseClient()
const tasks = ref({})

function goToTaskView() {

}

function goToListView() {

}

// Funktion, um Board-Details zu holen
async function fetchTasks() {
    const { data, error } = await supabase
        .from('tasks')
        .select('*')
        .eq('list_id', list.id)

    if (data) {
        tasks.value = data
    } else {
        console.error('Fehler beim Abrufen der Board-Details:', error?.message)
    }
}

const createTask = async ({ title }) => {
    if (title.trim() !== '') {
        const { data, error } = await supabase
            .from('tasks')
            .insert([
                { title: title, position: tasks.value.length, list_id: list.id }
            ])
            .select()
            .single()

        if (data) {
            tasks.value.push(data)
        } else if (error) {
            console.error('Fehler beim Erstellen der Liste:', error.message)
        }
    }
}

const deleteTask = async (itemId) => {
    const { data, error } = await supabase
        .from('tasks')
        .delete()
        .match({ id: itemId })
        .select()
        .single()

    if (data) {
        tasks.value = tasks.value.filter(task => task.id !== itemId)
    } else if (error) {
        console.error('Fehler beim Löschen des Tasks:', error.message)
    }
}

const deleteList = async (itemId) => {
    const { data, error } = await supabase
        .from('lists')
        .delete()
        .match({ id: itemId })
        .select()
        .single()

    if (data) {
        // Irgendwie lists zugreifen oder itemId übergeben an parent component
        //tasks.value = tasks.value.filter(list => list.id !== itemId)
    } else if (error) {
        console.error('Fehler beim Löschen der Liste:', error.message)
    }
}

onMounted(fetchTasks)
</script>