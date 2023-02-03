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
      this.newProject.date = Date()
      axios.post("http://localhost:3000/tasks", this.newProject).then(response => {
        console.log(response.data)
        this.newProject = response.data
        this.todoList.push(this.newProject)
        this.newProject = {}
      })
    },
    deleteTodo: function (todo) {
      axios.delete(`http://localhost:3000/tasks/${todo.id}`).then(response => {
        console.log(response.data)
        this.todoList.splice(this.todoList.indexOf(todo), 1)
      })
    },
    editTodo: function (todo) {
      axios.delete(`http://localhost:3000/tasks/${todo.id}`).then(response => {
        console.log(response.data)
        this.todoList.splice(this.todoList.indexOf(todo), 1)
      })
    },
  },
};
</script>

<template>
  <div class="add-todo">
    <h1>Welcome to your todo list! Please add some some projects!</h1>
    <p><b>Title: </b></p>
    <input type="text" v-model="this.newProject.title" placeholder="Title..." />
    <p><b>Description: </b></p>
    <textarea v-model="this.newProject.description" placeholder="Describe here..."> </textarea>
    <p><b>Deadline: </b></p>
    <input type="text" v-model="this.newProject.deadline" placeholder="Year-month-date..." />
    <p><button @click="postTodo(this.newProject)">Post Project</button></p>
  </div>

  <div class="todo-list" v-for="todo in this.todoList" v="if">
    <h1>
      <span> <button @click="editTodo(todo)">Edit</button></span>
      {{ todo.title }}
      <span> <button @click="deleteTodo(todo)"> Delete</button></span>
    </h1>
    <h2>{{ todo.description }}, due at {{ todo.deadline }}</h2>
  </div>
</template>

<style>

</style>