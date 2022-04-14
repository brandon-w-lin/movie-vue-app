<template>
  <div>
    <div>
      <h2>Create a new movie:</h2>
      <form>
        <p><input type="text" v-model="newMovieParams.title" placeholder="Title" /></p>
        <p><input type="text" v-model="newMovieParams.plot" placeholder="Plot" /></p>
        <p><input type="text" v-model="newMovieParams.year" placeholder="Year" /></p>
        <p><input type="text" v-model="newMovieParams.director" placeholder="Director" /></p>
      </form>
      <button v-on:click="movieCreate()">Submit</button>
      <br />
      <br />
      <div>
        <a href="http://localhost:8080/movies">Return to all movies page</a>
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
      newMovieParams: {},
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
    movieCreate: function () {
      axios.post("http://localhost:3000/movies.json", this.newMovieParams).then((response) => {
        this.movies.push(response);
      });
      this.newMovieParams.title = null;
      this.newMovieParams.year = null;
      this.newMovieParams.director = null;
      this.newMovieParams.plot = null;
    },
  },
};
</script>

<style>
#movie-show {
  width: 500px;
}

input {
  width: 450px;
}
</style>
