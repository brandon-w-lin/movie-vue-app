<template>
  <div>
    <!-- List of movies  -->
    <div>
      <div>
        <p>Title: {{ currentMovie.title }}</p>
        <p>Plot: {{ currentMovie.plot }}</p>
        <p>Year: {{ currentMovie.year }}</p>
        <p>Director: {{ currentMovie.director }}</p>
      </div>
      <button @click="moviesIndexRedirect()">Return to all movies</button>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      currentMovie: {},
    };
  },
  created: function () {
    this.moviesShow();
  },
  methods: {
    moviesShow: function () {
      axios.get("http://localhost:3000/movies/" + this.$route.params.id + ".json").then((response) => {
        this.currentMovie = response.data;
      });
    },
    moviesIndexRedirect: function () {
      this.$router.push("/movies");
    },
  },
};
</script>
