<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      todoList: {},
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
    }
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
  </div>
</template>

<style>

</style>