<template>
  <div class="home">
    <h1>{{ message }}</h1>
    {{ posts }}
  </div>
</template>

<style></style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      posts: [],
    };
  },
  created: function() {
    axios.get("https://jsonplaceholder.typicode.com/posts").then(response => {
      console.log(response.data);
      this.posts = response.data;
    });
  },
  methods: {},
};
</script>
