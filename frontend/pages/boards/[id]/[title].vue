<template>
    <div class="flex h-screen">
        <div class="flex-1 overflow-auto">
            <div class="bg-gray-200 dark:bg-gray-700 p-4 sticky top-0 z-10">
                <h1 class="text-xl font-bold text-gray-900 dark:text-white">{{ boardTitle }}</h1>
            </div>
            <div class="flex overflow-x-auto p-4 space-x-4">
                <TodoListCard v-for="list in lists" :list="list" />
                <!-- Neue Liste hinzufügen -->
                <div class="h-16 w-64 rounded font-semibold">
                    <CreateItemComponent name="Neue Liste" placeholder="Liste-Titel" @create="createList" />
                </div>
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
const boardTitle = ref(route.params.title)

const lists = ref({})

async function fetchLists() {
    const { data, error } = await supabase
        .from('lists')
        .select('*')
        .eq('board_id', boardId.value)

    if (data) {
        lists.value = data
    } else {
        console.error('Fehler beim Abrufen der Board-Details:', error?.message)
    }
}

const createList = async ({ title }) => {
  if (title.trim() !== '') {
    const { data, error } = await supabase
      .from('lists')
      .insert([
        { title: title, position: lists.value.length, board_id: boardId.value }
      ])
      .select()
      .single()

    if (data) {
      lists.value.push(data)
    } else if (error) {
      console.error('Fehler beim Erstellen der Liste:', error.message)
    }
  }
}

onMounted(fetchLists)
</script>
