<template>
  <aside class="flex flex-col h-screen bg-gray-50 dark:bg-gray-800" aria-label="Sidebar">
    <div class="py-4 px-3">
      <a href="/" class="flex items-center pl-2.5 mb-5">
        <div class="w-16 h-16 rounded-full overflow-hidden flex justify-center items-center bg-white">
          <img src="/Logo.webp" class="h-full w-full" alt="Logo">
        </div>
        <span class="ml-6 self-center text-xl font-semibold whitespace-nowrap dark:text-white">Todo App</span>
      </a>
      <div class="relative mt-6">
        <input type="text"
          class="block p-2 pl-10 w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white"
          placeholder="Suchen...">
        <!-- Suchicon und weitere Elemente -->
      </div>
    </div>
    <div class="flex-1 overflow-y-auto">
      <ul class="space-y-2">
        <li>
          <!-- Boards Liste -->
          <ListBoardComponent v-for="board in boards" :item="board" @click="goToBoardView" @delete="deleteBoard" />
          <!-- Neues Board hinzufügen -->
          <CreateItemComponent name="Neues Board" placeholder="Board-Titel" @create="createBoard" />
        </li>
      </ul>
    </div>
    <!-- Benutzerbereich am unteren Rand -->
    <div class="absolute bottom-0 py-4 px-3 bg-gray-50 dark:bg-gray-800">
      <a href="#"
        class="flex items-center p-2 text-base font-normal text-gray-900 rounded-lg dark:text-white hover:bg-gray-100 dark:hover:bg-gray-700">
        <span class="ml-3">Benutzername</span>
      </a>
    </div>

  </aside>
</template>

<script setup>
const supabase = useSupabaseClient();
const user = useSupabaseUser();

const boards = ref([])
const router = useRouter()

function goToBoardView(item) {
  console.log(item)
  router.push(`/boards/${item.id}/${item.title}`)
}

const fetchBoards = async () => {
  const { data, error } = await supabase
    .from('boards')
    .select('*')
    // Optional: Filtern, um nur Boards des angemeldeten Benutzers zu erhalten
    .eq('creator', user.value.id)

  if (data) {
    boards.value = data
  } else if (error) {
    console.error('Fehler beim Abrufen der Boards:', error.message);
  }
}

const createBoard = async ({ title }) => {
  if (title.trim() !== '') {
    const { data, error } = await supabase
      .from('boards')
      .insert([
        { title: title, position: boards.value.length, creator: user.value.id }
      ])
      .select()
      .single()

    if (data) {
      // Das Board wurde erfolgreich erstellt, fügen Sie es zur Liste hinzu
      boards.value.push(data)
    } else if (error) {
      console.error('Fehler beim Erstellen des Boards:', error.message)
    }
  }
}

const deleteBoard = async (itemId) => {
  console.log(itemId)
  const { data, error } = await supabase
    .from('boards')
    .delete()
    .match({ id: itemId })
    .select()
    .single()

  if (data) {
    // Entfernen Sie das Board aus der Liste
    boards.value = boards.value.filter(board => board.id !== itemId) //unklar
  } else if (error) {
    console.error('Fehler beim Löschen des Boards:', error.message)
  }
}

onMounted(fetchBoards)
</script>
