<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      todoList: {},
      newProject: {},
    };
  },
  created: function () {
    this.getList()
  },
  methods: {
    getList: function () {
      axios.get("http://localhost:3000/tasks").then(response => {
        console.log(response.data)
        this.todoList = response.data
      })
    },
    postTodo: function () {
      this.newProject.status = "Unfinished"
      this.newProject.date = Date.now()
      axios.post("http://localhost:3000/tasks", this.newProject).then(response => {
        console.log(response.data)
        this.todoList = response.data
      })
    },
  },
};
</script>

<template>
  <div class="add-todo">
    <h1>Welcome to your todo list! Please add some some projects!</h1>
    <p>Title: </p>
    <input type="text" v-model="this.newProject.title" placeholder="Title..." />
    <p>Description: </p>
    <textarea v-model="this.newProject.description" placeholder="Describe here..."> </textarea>
    <p>Deadline: </p>
    <input type="text" v-model="this.newProject.deadline" placeholder="Date..." />
  </div>

  <div class="todo-list">
    <h1 v-for="todo in this.todoList">{{ todo }}</h1>
  </div>
</template>

<style>

</style>