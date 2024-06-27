<template>
  <div v-if="!showInput">
    <a href="#" @click.prevent="showInput = true" class="flex items-center p-2 text-base font-normal bg-blue-500 hover:bg-blue-700 text-white py-1 px-2 rounded mt-3">
      <svg class="h-4 w-4 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6v12m6-6H6"></path>
      </svg>
      <span>{{ props.name }}</span>
    </a>
  </div>
  <div v-show="showInput">
    <input v-model="newItemTitle" @keyup.enter="createItem" @keyup.esc="cancelCreateItem"
      class="block p-2 w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white"
      :placeholder="props.placeholder" autofocus>
  </div>
</template>

<script setup>
const props = defineProps({
  name: String,
  placeholder: String,
})

const emit = defineEmits(["create"])

const showInput = ref(false)
const newItemTitle = ref('')

// Ereignisse emittieren
const createItem = () => {
  if (newItemTitle.value.trim() !== '') {
    emit("create", { title: newItemTitle.value })
    newItemTitle.value = '' // Zurücksetzen des Titels
    showInput.value = false // Verstecken des Eingabefeldes
  }
}

const cancelCreateItem = () => {
  newItemTitle.value = ''
  showInput.value = false
}

// defineDirective('autofocus', {
//   mounted(el) {
//     nextTick(() => {
//       el.focus();
//     });
//   },
// });
</script>