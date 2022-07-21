<script>
import Tasks from "../components/Tasks.vue";
import AddTask from "../components/AddTask.vue";

export default {
  name: "HomeView",
  props: {
    showAddTask: Boolean,
  },
  components: {
    Tasks,
    AddTask,
  },
  data() {
    return {
      tasks: [],
    };
  },
  methods: {
    async addTask(task) {
      const res = await fetch(`${process.env.VUE_APP_BASE_URL}/tasks`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(task),
      });

      const data = await res.json();

      this.tasks = [...this.tasks, data];
    },
    async deleteTask(id) {
      if (confirm("Are your sure ?")) {
        const res = await fetch(`${process.env.VUE_APP_BASE_URL}/tasks/${id}`, {
          method: "DELETE",
        });

        res.status == 200
          ? (this.tasks = this.tasks.filter((task) => task.id !== id))
          : alert("Error deleting task");
      }
    },
    async toggleReminder(id) {
      const taskToToggle = await this.fetchTask(id);
      const updatedTask = { ...taskToToggle, reminder: !taskToToggle.reminder };

      const res = await fetch(`${process.env.VUE_APP_BASE_URL}/tasks/${id}`, {
        method: "PUT",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(updatedTask),
      });

      const data = await res.json();

      res.status == 200
        ? (this.tasks = this.tasks.map((task) =>
            task.id === id ? { ...task, reminder: data.reminder } : task
          ))
        : alert("Error deleting task");
    },
    async fetchTasks() {
      const res = await fetch(`${process.env.VUE_APP_BASE_URL}/tasks`);
      <h1>This is an about page</h1>;
      const data = await res.json();
      return data;
    },
    async fetchTask(id) {
      const res = await fetch(`${process.env.VUE_APP_BASE_URL}/tasks/${id}`);

      const data = await res.json();

      return data;
    },
  },
  async created() {
    // this.tasks = [
    //   {
    //     id: "1",
    //     text: "Doctors Appointment",
    //     day: "March 5th at 2:30pm",
    //     reminder: true,
    //   },
    //   {
    //     id: "2",
    //     text: "Meeting with boss",
    //     day: "March 6th at 1:30pm",
    //     reminder: true,
    //   },
    //   {
    //     id: "3",
    //     text: "Food shopping",
    //     day: "March 7th at 2:00pm",
    //     reminder: false,
    //   },
    // ];
    this.tasks = await this.fetchTasks();
  },
};
</script>
<template>
  <div v-if="showAddTask">
    <AddTask @add-task="addTask" />
  </div>
  <Tasks
    @toggle-reminder="toggleReminder"
    @delete-task="deleteTask"
    :tasks="tasks"
  />
</template>
<style></style>
