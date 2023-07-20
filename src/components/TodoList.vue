<template>
    <div>
      <h1>Todo App</h1>
      <form @submit.prevent="addTask">
        <input type="text" v-model="newTask" placeholder="Add a new task">
        <button type="submit">Add Task</button>
      </form>
      <ul>
        <li v-for="(task, index) in tasks" :key="index">
          <span :class="{ 'task-completed': task.completed }">{{ task.title }}</span>
          <button @click="markAsCompleted(task.id)">Mark Completed</button>
          <button @click="deleteTask(task.id)">Delete</button>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        tasks: [],
        newTask: ''
      };
    },
    async created() {
      await this.fetchTasks();
    },
    methods: {
      async fetchTasks() {
        try {
          const response = await this.$axios.get('http://127.0.0.1:8000/api/todos');
          this.tasks = response.data;
        } catch (error) {
          console.error('Error fetching tasks:', error);
        }
      },
      async addTask() {
        if (this.newTask) {
          try {
            const response = await this.$axios.post('http://127.0.0.1:8000/api/todos', {
              title: this.newTask,
              completed: false
            });
            this.tasks.push(response.data);
            this.newTask = '';
          } catch (error) {
            console.error('Error adding task:', error);
          }
        }
      },
      async markAsCompleted(taskId) {
        try {
          await this.$axios.put(`http://127.0.0.1:8000/api/todos/${taskId}`, {
            completed: true
          });
          this.fetchTasks();
        } catch (error) {
          console.error('Error marking task as completed:', error);
        }
      },
      async deleteTask(taskId) {
        try {
          await this.$axios.delete(`http://127.0.0.1:8000/api/todos/${taskId}`);
          this.fetchTasks();
        } catch (error) {
          console.error('Error deleting task:', error);
        }
      }
    }
  };
  </script>
  
  <style>
  /* Your styles go here */
  </style>
  