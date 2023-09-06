<template>
    <div class="isolate bg-white px-6 py-24 sm:py-32 lg:px-8">
        <div class="mx-auto max-w-2xl text-center">
            <h2 class="text-3xl font-bold tracking-tight text-gray-900 sm:text-4xl">Task list </h2>
        </div>
        <form @submit.prevent="handleSubmit" class="mx-auto mt-16 max-w-xl sm:mt-20">
            <div class="grid grid-cols-1 gap-x-8 gap-y-6 sm:grid-cols-2">
                <div class="sm:col-span-2">
                    <label for="name" class="block text-sm font-semibold leading-6 text-gray-900">Task name</label>
                    <div class="mt-2.5">
                        <input v-model="form.title" type="text" name="name" id="name" autocomplete="organization"
                            class="block w-full rounded-md border-0 px-3.5 py-2 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6">
                    </div>
                </div>
                <div class="sm:col-span-2">
                    <label for="status" class="block text-sm font-semibold leading-6 text-gray-900">Status</label>
                    <div class="mt-2.5">
                        <select v-model="form.status" name="status" id="status"
                            class="block w-full rounded-md border-0 px-3.5 py-2 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6">
                            <option value="backlog">Backlog</option>
                            <option value="in-progress">In Progress</option>
                            <option value="completed">Completed</option>
                        </select>
                    </div>
                </div>
                <div class="sm:col-span-2">
                    <div class="mt-2.5 flex space-x-2">
                        <label class="block text-sm font-semibold leading-6 block w-1/2 text-gray-900">Deadline Date</label>
                        <label class="block text-sm font-semibold leading-6 block w-1/2 text-gray-900">Deadline Time</label>
                    </div>
                    <div class="mt-2.5 flex space-x-2">
                        <input v-model="form.deadlinedate" type="date" name="deadlineDate" id="deadlineDate"
                            class="block w-1/2 rounded-md border-0 px-3.5 py-2 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6">
                        <input v-model="form.deadlinetime" type="time" name="deadlineTime" id="deadlineTime"
                            class="block w-1/2 rounded-md border-0 px-3.5 py-2 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6">
                    </div>
                </div>

                <div class="sm:col-span-2">
                    <label for="description" class="block text-sm font-semibold leading-6 text-gray-900">Task
                        description</label>
                    <div class="mt-2.5">
                        <textarea v-model="form.description" name="description" id="description" rows="4"
                            class="block w-full rounded-md border-0 px-3.5 py-2 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"></textarea>
                    </div>
                </div>

            </div>
            <div class="mt-10">
                <button type="submit"
                    class="block w-full rounded-md bg-indigo-600 px-3.5 py-2.5 text-center text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600">
                    {{ mode === 'create' ? 'Create' : 'Edit' }}
                </button>
            </div>
            <div v-if="mode === 'edit'" class="mt-4">
                <button @click.prevent="handleDelete"
                    class="block w-full rounded-md bg-red-600 px-3.5 py-2.5 text-center text-sm font-semibold text-white shadow-sm hover:bg-red-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-red-600">
                    Delete
                </button>
            </div>
        </form>
    </div>
</template>

<script setup>
const supabase = useSupabaseClient();
const { mode, taskData } = defineProps({
    mode: {
        type: String,
        default: 'create'
    },
    taskData: {
        type: Object,
        default: () => ({})
    }
});

const form = reactive({
    title: taskData.title || '',
    deadlinedate: taskData.deadlinedate || null,
    deadlinetime: taskData.deadlinetime || null,
    description: taskData.description || '',
    status: taskData.status || 'backlog'
});

const handleSubmit = async () => {
    const formData = { ...form };
    if (mode === 'create') {
        const { error } = await supabase.from('tasks').insert([formData]);
        if (error) console.error(error);
    } else if (mode === 'edit') {
        // Handle the editing logic here
        const { error } = await supabase
            .from('tasks')
            .update(formData)
            .eq('id', taskData.id);
        if (error) console.error(error);
    }
    navigateTo('/');  // Redirect to root
}

const handleDelete = async () => {
    if (confirm("Are you sure to delete task "+taskData.title + "?")) {
        const { error } = await supabase
            .from('tasks')
            .delete()
            .eq('id', taskData.id);
        if (error) {
            console.error(error);
        } else {
            navigateTo('/');  // Redirect to root after deletion
        }
    }
}

</script>
