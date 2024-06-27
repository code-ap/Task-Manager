<template>
    <div>
        <a href="#" class="flex justify-between items-center p-2 text-base font-normal text-gray-900 rounded-lg dark:text-white">
            <!-- Flex-Container für den Text, der wächst, um den verfügbaren Platz zu füllen -->
            <div @click.prevent="clickItem(item)" class="flex-grow hover:bg-gray-100 dark:hover:bg-gray-700 rounded-lg p-1">
                <span class="ml-3">{{ item.title }}</span>
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

const emit = defineEmits(["click", "delete"])

// Ereignisse emittieren
const clickItem = (item) => {
    emit("click", item)
}

const deleteItem = (itemId) => {
    emit("delete", itemId)
}
</script>