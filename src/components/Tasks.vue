<script>
import Task from "./Task.vue";

export default {
  name: "TasksComponent",
  props: {
    tasks: Array,
  },
  components: {
    Task,
  },
};
</script>

<template>
  <div v-for="task in tasks" :key="task.id">
    <Task
      @toggle-reminder="$emit('toggle-reminder', task.id)"
      @delete-task="$emit('delete-task', task.id)"
      :task="task"
    />
  </div>
</template>

<style></style>
