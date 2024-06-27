<template>
    <div class="flex h-screen">
        <!-- Board Container -->
        <div class="flex-1 overflow-auto">
            <!-- Board Header -->
            <div class="bg-gray-200 dark:bg-gray-700 p-4 sticky top-0 z-10">
                <h1 class="text-xl font-bold text-gray-900 dark:text-white">Board Titel</h1>
                <!-- Konfigurationsoptionen hier -->
            </div>
            <!-- Listen Container -->
            <div class="flex overflow-x-auto p-4 space-x-4">
                <TodoListCard/>
            </div>
        </div>
    </div>
</template>

<script setup>
// JavaScript Logik für Interaktivität
</script>

<style scoped>
/* Zusätzliches Styling falls nötig */
</style>
