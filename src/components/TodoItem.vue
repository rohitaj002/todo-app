<template>
    <li>
      <span :class="{ 'task-completed': task.completed }">{{ task.title }}</span>
      <button @click="$emit('mark-as-completed')">Mark Completed</button>
      <button @click="$emit('delete-task')">Delete</button>
    </li>
  </template>
  
  <script>
  export default {
    props: {
      task: {
        type: Object,
        required: true
      }
    }
  };
  </script>
  