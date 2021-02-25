<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="post in redditPosts" v-bind:key="post.data.name">
      {{ post.data.title }}
    </div>
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
      redditPosts: [],
    };
  },
  created: function() {
    axios.get("https://jsonplaceholder.typicode.com/posts").then(response => {
      console.log(response.data);
      this.posts = response.data;
    });
    axios.get("/api/redditstuff").then(response => {
      console.log(response.data);
      this.redditPosts = response.data.children;
    });
  },
  methods: {},
};
</script>
