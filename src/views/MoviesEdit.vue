<template>
  <div>
    <!-- List of movies  -->
    <div>
      <div>
        <form>
          <p>
            Title:
            <input v-model="currentMovie.title" type="text" placeholder="Title" />
          </p>
          <p>
            Plot:
            <input v-model="currentMovie.plot" type="text" placeholder="Plot" />
          </p>
          <p>
            Year:
            <input v-model="currentMovie.year" type="text" placeholder="Year" />
          </p>
          <p>
            Director:
            <input v-model="currentMovie.director" type="text" placeholder="Director" />
          </p>
        </form>
        <button @click="moviesUpdate()">Submit changes</button>
      </div>
      <div>
        <button @click="moviesDestroy()">Delete movie</button>
      </div>
      <button @click="moviesShowRedirect()">Return to movie page</button>
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
    moviesUpdate: function () {
      axios
        .patch("http://localhost:3000/movies/" + this.$route.params.id + ".json", this.currentMovie)
        .then((response) => {
          this.currentMovie = response.data;
        });
    },
    moviesDestroy: function () {
      axios.delete("http://localhost:3000/movies/" + this.$route.params.id + ".json").then((response) => {
        console.log(response.data);
      });
      this.moviesIndexRedirect();
    },
    moviesIndexRedirect: function () {
      this.$router.push("/movies");
    },
    moviesShowRedirect: function () {
      this.$router.push("/movies/" + this.currentMovie.id);
    },
  },
};
</script>
