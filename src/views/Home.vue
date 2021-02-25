<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <a
      v-if="!spotifyAccessToken"
      href="https://accounts.spotify.com/authorize?client_id=9c1b880635d5464d811647e65e3bdb40&response_type=code&redirect_uri=http://localhost:8080"
    >
      Log in with Spotify
    </a>
    <div v-if="spotifyAccessToken">
      <input type="text" v-model="artistSearch" />
      <button v-on:click="getSpotifyInfo()">Get Spotify search</button>
    </div>
    <div v-for="artist in artists" v-bind:key="artist.id">
      <img v-if="artist.images[0]" v-bind:src="artist.images[0].url" alt="" />
    </div>
    <br />
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
      spotifyAccessToken: null,
      artistSearch: "",
      artists: [],
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
    var code = this.$route.query.code;
    if (code) {
      console.log("gonna get spotify access", code);
      axios.post("/api/spotifystuff", { code: code }).then(response => {
        console.log(response);
        this.spotifyAccessToken = response.data.message.access_token;
        localStorage.setItem("spotifyAccessToken", this.spotifyAccessToken);
        this.$router.push("/");
      });
    } else {
      this.spotifyAccessToken = localStorage.getItem("spotifyAccessToken");
      console.log("you haven't started the oauth process, no code man");
    }
  },
  methods: {
    getSpotifyInfo: function() {
      console.log(this.spotifyAccessToken);
      axios
        .post("/api/spotify_profile", { artist: this.artistSearch, access_token: this.spotifyAccessToken })
        .then(response => {
          console.log(response.data);
          this.artists = response.data.artists.items;
        });
    },
  },
};
</script>
