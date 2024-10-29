<template>
  <div class="uk-card uk-card-default uk-width-1-2@m card-template">
    <div class="uk-card-header">
      <div class="uk-grid-small uk-flex-middle" uk-grid>
        <div class="uk-width-auto">
          <img class="uk-border-circle" width="40" height="40" :src="movie.poster_path" alt="Avatar" />
        </div>
        <div class="uk-width-expand">
          <h3 class="uk-card-title uk-margin-remove-bottom">{{ movie.original_title }}</h3>

          <p class="uk-text-meta uk-margin-remove-top">
            <time :datetime="movie.release_date">{{ movie.release_date }}</time>
          </p>
        </div>
      </div>
    </div>
    <div class="uk-card-body">
      <p>{{ movie.overview }}</p>
    </div>
    <div class="uk-card-footer">
      <div>
        <span>Original Language: {{ movie.original_language }}</span>
      </div>
      <div>
        <span>Note: {{ movie.popularity }}</span>
      </div>
    </div>
  </div>
  <div></div>
  <div></div>
  <div>
    <span></span>
  </div>
</template>
<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "MovieCompo",
  beforeMount(){
    this.buildImage();
  },
  data() {
    return {
      imagePath: "",
    };
  },
  props: ["movie"],
  methods: {
    buildImage() {
      console.log(this.movie.poster_path);
      const path = `https://image.tmdb.org/t/p/w500/${this.movie.poster_path}.jpg`;
      console.log(path);
    },
  },
});
</script>
<style scoped lang="scss">
.card-template {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  transition: all 450ms ease-in-out;
  &:hover {
    background-color: darken(lightgrey, 10%);
    -webkit-box-shadow: 11px 11px 27px 2px rgba(0, 0, 0, 0.21);
    box-shadow: 11px 11px 27px 2px rgba(0, 0, 0, 0.21);
  }
}
</style>
