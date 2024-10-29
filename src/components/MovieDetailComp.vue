<template>
  <div class="uk-container">
    <div>
      <HeaderComp />
    </div>
    <div class="arrow"><RouterLink to="/">Retour</RouterLink></div>
    <div>
      <div class="">
        <div>
          <div class="">
            <img class="uk-border-circle" width="40" height="40" :src="image" alt="Movie Poster" />
          </div>
          <div class="">
            <h3 class="">Title</h3>
            <p class="">
              <time datetime="2016-04-01T19:00">Release Date: {{ movie.release_date }}</time>
            </p>
          </div>
        </div>
      </div>
      <div class="">
        <p>{{ movie.overview }}</p>
      </div>
      <div class="">
        <a @click="downloadFile" class="uk-button uk-button-text">Download me as PDF</a>
      </div>
    </div>
    <!-- <div>
      <span>{{ movie.original_title }}</span>
      <p>{{ movie.overview }}</p>
      <img :src="image" alt="Affiche du film" />
      <span>{{ movie.original_title }}</span>
      <span>{{ movie.original_title }}</span>
      <span>{{ movie.original_title }}</span>
    </div> -->
  </div>
</template>

<script>
import HeaderComp from "@/components/HeaderComp.vue";
// import SearchComp from "@/components/SearchComp.vue";
// import MovieDetailComp from "@/components/MovieDetailComp.vue";

import axios from "axios";
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: "MovieDetailComp",

  components: {
    HeaderComp,
    // SearchComp,
    // MovieDetailComp,
  },
  props: ["movie"],
  watch: {
    movie: {
      immediate: true,
      handler() {
        this.buildImage();
      },
    },
  },
  async mounted() {
    await this.buildImage(this.movie.poster_path);
  },
  data() {
    return {
      image: "",
      movieData: this.movie,
    };
  },
  methods: {
    async buildImage(posterPath) {
      if (posterPath) {
        const imagePath = `https://image.tmdb.org/t/p/w500/${posterPath}`;
        this.image = imagePath;
      } else {
        console.warn("Aucun poster_path trouvé pour ce film.");
      }
    },
    async downloadFile() {
      try {
        const response = await axios.get(`http://localhost:3000/movies-download/${this.$route.params.id}`, { responseType: "blob" });
        
        // Créer un URL temporaire pour le fichier
        const url = window.URL.createObjectURL(new Blob([response.data], { type: "application/pdf" }));
        const link = document.createElement("a");
        link.href = url;
       
        // Nommer le fichier avec le titre récupéré depuis le backend si possible
        const contentDisposition = response.headers["content-disposition"];
        const fileName = contentDisposition ? contentDisposition.split("filename=")[1] : `${this.movie.original_title}.pdf`;
        link.setAttribute("download", fileName);
       
        // Ajouter le lien au DOM et cliquer pour télécharger
        document.body.appendChild(link);
        link.click();

        // Nettoyer
        link.remove();
        window.URL.revokeObjectURL(url);
      } catch (error) {
        console.error(error);
      }
      console.log(this.$route.params.id);
      // await axios.get(`http://localhost:3000/movies-download/996129`);
    },
  },
};
</script>
<style>
.arrow {
  background-color: red;
  border-radius: 50%;
  height: 50px;
  width: 50px;
}
</style>
