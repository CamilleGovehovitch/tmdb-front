<template>
  <div class="home">
    <div class="uk-container header">
      <HeaderComp />
    </div>
    <div class="search-container">
      <SearchComp @searchQuerry="searchMovie" />
      <div class="movie-list-container" v-if="this.movieList.length > 0">
        <ul class="movie-list">
          <li v-for="movie in movieList" :key="movie.id">
            <a @click.prevent="moveToDetail(movie.id)">
              <div class="uk-card uk-card-body">
                <MovieComp :movie="movie" />
              </div>
            </a>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import HeaderComp from "@/components/HeaderComp.vue";
import SearchComp from "@/components/SearchComp.vue";
import MovieComp from "@/components/MovieComp.vue";

import axios from "axios";
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: "HomeView",
  components: {
    HeaderComp,
    SearchComp,
    MovieComp,
  },
  data() {
    return {
      query: "",
      movieList: [],
    };
  },
  methods: {
    async searchMovie(payload) {
      this.query = payload.query;
      const movieResponse = await axios.get(`http://localhost:3000/movies-search?query=${this.query}`);
      this.movieList = movieResponse.data;
      // console.log(this.movieList.poster_path, 'PATH');
    },
    async moveToDetail(movieId) {
      // console.log(movieId);
      this.$router.push({ name: "MovieDetailView", params: { id: movieId } });
    },
  },
};
</script>
<style>
.home {
  .uk-container {
    padding: 0px;
    max-width: 100%;
  }
  .search-container {
    .movie-list-container {
      .movie-list {
        padding: 10px 20px;
        height: 500px;
        overflow: scroll;
        list-style: none;
        li {
          list-style: none;
          margin-bottom: 15px;
          /* border-bottom: solid 1px black; */
          a {
            text-decoration: none;
            color: black;
            .uk-card {
              display: flex;
              flex-direction: column;
              align-items: center;
            }
          }
        }
      }
    }
  }
}
</style>
