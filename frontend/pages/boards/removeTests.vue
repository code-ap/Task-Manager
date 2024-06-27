<template>
    <div class="flex h-screen">
        <div class="flex-1 overflow-auto">
            <div class="bg-gray-200 dark:bg-gray-700 p-4 sticky top-0 z-10">
                <h1 class="text-xl font-bold text-gray-900 dark:text-white">{{ boardDetails.title }}</h1>
            </div>
            <div v-for="list in lists" class="flex overflow-x-auto p-4 space-x-4">
                <TodoListCard :list="list"/>
            </div>
        </div>
    </div>
</template>

<script setup>
definePageMeta({
  layout: 'side-menu'
})

const supabase = useSupabaseClient()
const route = useRoute()
const boardId = ref(route.params.id)
const boardDetails = ref({}) // Initialisiere ein leeres Objekt für die Board-Details
const lists = ref({})

// Funktion, um Board-Details zu holen
async function fetchBoardDetails() {
    const { data, error } = await supabase
        .from('boards')
        .select('*')
        .eq('id', boardId.value)
        .single() // Da du erwartest, nur ein Board zurückzubekommen

    if (data) {
        boardDetails.value = data
        fetchLists()
    } else {
        console.error('Fehler beim Abrufen der Board-Details:', error?.message)
    }
}

async function fetchLists() {
    const { data, error } = await supabase
        .from('lists')
        .select('*')
        .eq('board_id', boardId.value)

    if (data) {
        lists.value = data
        console.log((lists))
    } else {
        console.error('Fehler beim Abrufen der Board-Details:', error?.message)
    }
}

onMounted(fetchLists)
</script>
