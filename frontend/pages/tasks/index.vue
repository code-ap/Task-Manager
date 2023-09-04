<template>
  <div class="p-6 flex space-x-6">
    <div v-for="(tasksList, category) in tasksData" :key="category" :class="getColumnClass(category)">
      <h2 class="text-xl font-bold mb-4">{{ category }}</h2>
      <TaskCard v-for="task in tasksList" :key="task.id" :task="task" />
    </div>
  </div>
</template>

<script>
import tasksData from '~/assets/tasksData.json'
import TaskCard from '~/components/TaskCard.vue'

export default {
  data() {
    return {
      tasksData
    }
  },
  components: {
    TaskCard
  },
  methods: {
    getColumnClass(category) {
      if (category === 'doneTasks') return 'flex-1 bg-gray-100 p-4 rounded-lg';
      if (category === 'todayTasks') return 'flex-1 bg-gray-200 p-4 rounded-lg';
      if (category === 'backlogTasks') return 'flex-1 bg-gray-300 p-4 rounded-lg';
    }
  }
}
</script>
