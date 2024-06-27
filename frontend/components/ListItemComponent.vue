<template>
    <div>
        <a href="#"
            class="flex justify-between items-center p-2 text-base font-normal text-gray-900 rounded-lg dark:text-white">
            <!-- Flex-Container für den Text, der wächst, um den verfügbaren Platz zu füllen -->
            <div class="flex-grow hover:bg-gray-100 dark:hover:bg-gray-700 rounded-lg p-1">
                <span v-if="!isEditing" @click="enableEditing" class="ml-3">{{ item.title }}</span>
                <input v-show="isEditing" v-model="editedTitle" @keyup.enter="updateTitle" @keyup.esc="cancelEditing"
                    @blur="cancelEditing" class="ml-3 p-1 rounded" autofocus>
            </div>
            <!-- Flex-Container für das SVG, ohne flex-grow, damit es nur den nötigen Platz einnimmt -->
            <div @click="deleteItem(item.id)"
                class="hover:bg-gray-100 dark:hover:bg-gray-700 rounded-lg p-1 w-8 h-8 flex items-center justify-center">
                <img src="/Trashbin.svg" />
            </div>
        </a>
    </div>
</template>

<script setup>
const props = defineProps({
    item: {
        type: Object,
        required: true
    }
})

const emit = defineEmits(["click", "delete", "update"])

// State to manage edit mode and edited title
const isEditing = ref(false);
const editedTitle = ref('');

// Enable editing mode
const enableEditing = () => {
  isEditing.value = true;
  editedTitle.value = props.item.title;
};

// Update title logic
const updateTitle = () => {
  emit("update", { ...props.item, title: editedTitle.value }); // You might need to adjust this based on how your backend expects to receive updates
  isEditing.value = false;
};

// Cancel editing and revert changes
const cancelEditing = () => {
  isEditing.value = false;
  editedTitle.value = ''; // Clear the temporary title
};

// Ereignisse emittieren
const clickItem = (item) => {
    emit("click", item)
}

const deleteItem = (itemId) => {
    emit("delete", itemId)
}
</script>