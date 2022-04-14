<template>
  <div>
    <!-- List of movies  -->
    <div>
      <h2>All movies:</h2>
      <div v-for="movie in movies" :key="movie.id">
        {{ movie.title }}
        <p><button @click="moviesShowRedirect(movie)">More Info</button></p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      movies: [],
    };
  },
  created: function () {
    this.moviesIndex();
  },
  methods: {
    moviesIndex: function () {
      axios.get("http://localhost:3000/movies.json").then((response) => {
        this.movies = response.data;
      });
    },
    moviesShowRedirect: function (movie) {
      this.$router.push("/movies/" + movie.id);
    },
  },
};
</script>
