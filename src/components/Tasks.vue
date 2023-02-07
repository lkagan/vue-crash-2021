<template>
  <div v-for="task in tasks">
    <Task @delete-task="$emit('delete-task', task.id)" :task="task" />
  </div>
</template>

<script>
import Task from "@/components/Task.vue";

export default {
  name: "Tasks",
  components: { Task },
  props: {
    tasks: Array
  }
}
</script>

<style scoped>

</style>