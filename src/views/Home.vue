<template>
    <div v-if="showAddTask">
      <AddTask @add-task="addTask"/>
    </div>
    <Tasks
        :tasks="tasks"
        @delete-task="deleteTask"
        @toggle-reminder="toggleReminder"
    />
</template>

<script>
import Tasks from "@/components/Tasks.vue";
import AddTask from "@/components/AddTask.vue";

export default {
  name: 'Home',

  props: {
    showAddTask: {
      type: Boolean,
      required: true
    }
  },

  data() {
    return {
      tasks: [],
    }
  },

  methods: {
    async deleteTask(id) {
      if (confirm('Are you sure?')) {
        await fetch(`/api/tasks/${ id }`, {
          method: 'DELETE',
          headers: {
            'content-type': 'application/json',
          }
        })
            .then((res) => {
              if (res.ok) {
                this.tasks = this.tasks.filter(task => task.id != id);
              } else {
                alert('Delete failed');
              }
            })
      }
    },

    async toggleReminder(id) {
      const task = await this.fetchTask(id);
      task.reminder = !task.reminder

      fetch(`/api/tasks/${ id }`, {
        method: 'PUT',
        headers: {
          'content-type': 'application/json'
        },
        body: JSON.stringify(task),
      })
          .then(res => res.json())
          .then(updatedTask => {
            this.tasks = this.tasks.map(task => {
              return task.id === id ? updatedTask : task;
            })
          });
    },

    async addTask(task) {
      return await fetch('api/tasks', {
        method: 'POST',
        headers: {
          'content-type': 'application/json',
        },
        body: JSON.stringify(task)
      })
          .then((res) => res.json())
          .then(task => this.tasks.push(task));
    },

    async fetchTasks() {
      return await fetch('api/tasks')
          .then(res => res.json());
    },

    async fetchTask(id) {
      return await fetch(`api/tasks/${ id }`)
          .then(res => res.json());
    }
  },

  async created() {
    this.tasks = await this.fetchTasks();
  },

  components: {
    AddTask,
    Tasks,
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: 'Poppins', sans-serif;
}

.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}

.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}

.btn:focus {
  outline: none;
}

.btn:active {
  transform: scale(0.98);
}

.btn-block {
  display: block;
  width: 100%;
}
</style>
