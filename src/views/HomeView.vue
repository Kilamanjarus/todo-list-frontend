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
        // console.log(response.data)
        this.todoList = response.data
        this.todoList.forEach(list => {
          list.editMode = false
        });
      })
    },
    updateTodo: function (todo) {
      axios.get(`http://localhost:3000/tasks/${todo.id}`).then(response => {
        // console.log(response.data)
        this.todoList[todo] = response.data
        this.todoList.editMode = false
      })
    },
    postTodo: function () {
      this.newProject.status = "Unfinished"
      this.newProject.date = Date()
      axios.post("http://localhost:3000/tasks", this.newProject).then(response => {
        // console.log(response.data)
        this.newProject = response.data
        this.todoList.push(this.newProject)
        this.newProject = {}
      })
    },
    deleteTodo: function (todo) {
      axios.delete(`http://localhost:3000/tasks/${todo.id}`).then(response => {
        // console.log(response.data)
        this.todoList.splice(this.todoList.indexOf(todo), 1)
      })
    },
    startEdit: function (todo) {
      todo.editMode = !todo.editMode
    },
    finishEdit: function (todo) {
      todo.editMode = false
      axios.patch(`http://localhost:3000/tasks/${todo.id}`, todo).then(response => {
        // console.log(response.data)
      }).then(this.updateTodo(todo))
    },
  },
};
</script>

<template>
  <!-- Add Todo -->
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

  <!-- Todo List -->
  <div class="todo-list" v-for="todo in this.todoList">
    <!-- Normal Mode -->
    <h1 v-if="todo.editMode != true || todo.editMode == undefined">
      <span> <button @click="startEdit(todo)">Edit</button></span>
      {{ todo.title }}
      <span> <button @click="deleteTodo(todo)"> Delete</button></span>
    </h1>
    <!-- Edit Mode -->
    <h1 v-if="todo.editMode == true">
      <span> <button @click="finishEdit(todo)">Finish Editing</button></span>
      <input type="text" v-model="todo.title" />
      <span> <button @click="deleteTodo(todo)"> Delete</button></span>
    </h1>
    <h2 v-if="todo.editMode != true">{{ todo.description }}, due at {{ todo.deadline }}</h2>
    <h2 v-if="todo.editMode == true">
      <textarea v-model="todo.description"> </textarea>
      <input type="text" v-model="todo.deadline" />
    </h2>
  </div>
</template>

<style>

</style>