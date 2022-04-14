<template>
  <div>
    <h1>Welcome to the movie page</h1>
    <!-- Create a new movie  -->
    <div>
      <button @click="modalMovieCreateOpen()">Create a new movie</button>
      <dialog id="movie-create">
        <div>
          <h2>Create a new movie:</h2>
          <form>
            <p><input type="text" v-model="newMovieParams.title" placeholder="Title" /></p>
            <p><input type="text" v-model="newMovieParams.plot" placeholder="Plot" /></p>
            <p><input type="text" v-model="newMovieParams.year" placeholder="Year" /></p>
            <p><input type="text" v-model="newMovieParams.director" placeholder="Director" /></p>
          </form>
          <button v-on:click="movieCreate()">Submit</button>
          <button v-on:click="modalMovieCreateClose()">Close</button>
        </div>
      </dialog>
    </div>

    <!-- List of movies  -->
    <div>
      <h2>All movies:</h2>
      <div v-for="movie in movies" :key="movie.id">
        {{ movie.title }}

        <!-- Show / Edit Movies  -->
        <button v-on:click="modalMovieShowOpen(movie)">More details</button>
        <dialog id="movie-show">
          <form>
            <p><input type="text" v-model="currentMovie.title" placeholder="Title" /></p>
            <p><input type="text" v-model="currentMovie.plot" placeholder="Plot" /></p>
            <p><input type="text" v-model="currentMovie.year" placeholder="Year" /></p>
            <p><input type="text" v-model="currentMovie.director" placeholder="Director" /></p>
          </form>
          <button @click="movieUpdate()">Submit Changes</button>
          <button @click="movieDelete()">Delete Movie</button>
          <br />
          <button @click="modalMovieShowClose()">Close</button>
        </dialog>
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
      currentMovie: {},
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
      this.moviesIndex();
    },
    movieUpdate: function () {
      axios
        .patch("http://localhost:3000/movies/" + this.currentMovie.id + ".json", this.currentMovie)
        .then((response) => {
          console.log(response.data);
          this.moviesIndex();
        })
        .catch((error) => console.log(error.response));
      document.getElementById("movie-show").close();
    },
    movieDelete: function () {
      axios
        .delete("http://localhost:3000/movies/" + this.currentMovie.id + ".json", this.currentMovie)
        .then((response) => {
          console.log(response.data);
          this.moviesIndex();
        })
        .catch((error) => console.log(error.response));
      document.getElementById("movie-show").close();
    },
    modalMovieShowOpen: function (movie) {
      this.currentMovie = movie;
      document.getElementById("movie-show").showModal();
    },
    modalMovieShowClose: function () {
      document.getElementById("movie-show").close();
    },
    modalMovieCreateOpen: function () {
      document.getElementById("movie-create").showModal();
    },
    modalMovieCreateClose: function () {
      document.getElementById("movie-create").showModal();
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
