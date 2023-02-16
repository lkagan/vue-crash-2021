<template>
  <header>
    <h1>{{ title }}</h1>
    <Button
        @btnClick="$emit('on-click-add-task')"
        :text="showAddTask ? 'Close' : 'Add Task'"
        :color="showAddTask ? 'red' : 'green'"
        v-show="homePage"
    ></Button>
  </header>
</template>

<script>
import Button from './Button.vue'

export default {
  name: "Header",

  components: {
    Button
  },

  props: {
    title: String,
    showAddTask: Boolean,
  },

  computed: {
    homePage() {
      return this.$route.path === '/';
    }
  }
}
</script>

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
</style>