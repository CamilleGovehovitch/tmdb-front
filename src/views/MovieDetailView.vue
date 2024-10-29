<template>
  <div class="">
    <MovieDetailComp :movie="movieDetail"/>
  </div>
</template>

<script>
import MovieDetailComp from "@/components/MovieDetailComp.vue";

import axios from "axios";
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: "MovieDetailView",
  components: {
    MovieDetailComp,
  },
  created() {
    this.getDetails(this.$route.params.id);
  },
  data() {
    return {
      query: "",
      movieDetail: {},
      errorMsg: "",
      imagePath: "",
    };
  },
  props:['movie'],
  methods: {
    async getDetails(movieId) {
      try {
        if (typeof movieId !== "string") {
          this.errorMsg = "Une erreur est survenue, l'id est invalide";
          return;
        }
        const movieDetailResponse = await axios.get(`http://localhost:3000/movie/${movieId}`);

        this.movieDetail = movieDetailResponse.data;
        // const imagePath = `https://image.tmdb.org/t/p/w500/${this.movie.poster_path}.jpg`;
        this.imagePath = `https://image.tmdb.org/t/p/w500/${this.imagePath}.jpg`;
      } catch (error) {
        console.error(error);
      }
    },
  },
};
</script>
<style></style>
