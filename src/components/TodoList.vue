<template>
    <div class="todo-template">
      <input class="checkbox-all" type="checkbox" v-model="showall"> Show all Tasks
      <form @submit.prevent="addTask">
        <img src="/images/profile.png" class="profile_img" />
        <input type="text" v-model="newTask" placeholder="Project # To Do">
        <button type="submit">Add Task</button>
      </form>
      <table>
        <tr v-for="(task, index) in tasks" :key="index">
          <th v-if="!task.completed || showall">
            <input class="completed_checkbox" type="checkbox" @click="markAsCompleted(task.id)" :checked="task.completed">
          </th>
          <th v-if="!task.completed || showall">
            <span :class="{ 'task-completed': task.completed }">{{ task.title }}</span>
            <span class="update_message" >  updated few seconds ago</span>
          </th>
          <th v-if="!task.completed || showall">
            <img src="/images/profile.png" class="profile_img" />
          </th>
          <th v-if="!task.completed || showall">
            <img src="/images/delete.png" @click="deleteTask(task.id)" alt="Delete" class="delete-button"/>
          </th>
        </tr>
      </table>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        tasks: [],
        newTask: '',
        showall:false
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
            completed: !this.tasks.find(task => task.id === taskId).completed,
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
  

  