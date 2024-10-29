<template>
  <div class="title-search-container">
    <div class="uk-heading-medium title-container">
      <h3 class="uk-heading-line uk-text-left"><span>Search your movie</span></h3>
    </div>
    <div class="search-container">
      <form @submit.prevent="emitSearchQuery">
        <div class="uk">
          <div class="uk-inline input-container">
            <span class="uk-form-icon uk-form-icon-flip" uk-icon="icon: search"></span>
            <input v-model="searchQuery" class="uk-input search-input" type="search" placeholder="Name, Genre..." id="searchInput" />
          </div>
          <button class="uk-button uk-button-default btn-submit" type="submit">Search</button>
        </div>
        <div v-if="errors.state === true">
          <span class="error">{{ errors.message }}</span>
        </div>
      </form>
      <div class="download-container">
        <button @click="downloadMovieList" class="uk-button uk-button-default uk-button-small download-button">Download the list</button>
      </div>
    </div>
  </div>
</template>

<script>
import { defineComponent } from "vue";
import axios from "axios";

export default defineComponent({
  name: "SearchCompo",
  data() {
    return {
      searchQuery: "",
      errors: {
        state: false,
        message: "Field can not remained empty.",
      },
    };
  },
  methods: {
    emitSearchQuery() {
      const searchInput = document.getElementById("searchInput");
      if (this.searchQuery === "") {
        searchInput.classList.add("uk-form-danger");
        this.errors.state = true;
      } else {
        searchInput.classList.remove("uk-form-danger");
        this.errors.state = false;
        this.$emit("searchQuerry", { query: this.searchQuery });
      }
    },
    async downloadMovieList() {
      try {
        const response = await axios.get(`http://localhost:3000/movies-search-download`, { responseType: "blob" });
        // Créer un URL temporaire pour le fichier
        const url = window.URL.createObjectURL(new Blob([response.data], { type: "application/pdf" }));
        const link = document.createElement("a");
        link.href = url;
        // Nommer le fichier avec le titre récupéré depuis le backend si possible
        const contentDisposition = response.headers["content-disposition"];
        const fileName = contentDisposition ? contentDisposition.split("filename=")[1] : `${this.searchQuery}.pdf`;
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
    },
  },
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.title-search-container {
  padding: 20px 15px;
  .title-container {
    h3 {
      font-size: 18px;
    }
  }
  .search-container {
    form {
      margin-bottom: 20px;
      .input-container {
        margin-right: 20px;
      }
      text-align: left;
      .search-input {
        // border: none;
        border-radius: 15px;
        color: black;
        // margin-bottom: 10px;
      }
      .btn-submit {
        border-radius: 10px;
        padding: 0px 10px;
        transition: all 450ms ease-in-out;
        &:hover {
          background-color: lightblue;
          font-weight: bold;
        }
      }
    }
  }
  .download-container {
    text-align: left;
    button {
      border-radius: 15px;
    }
  }
}
.error {
  color: red;
}
@media only screen and (min-width: 768px) {
}
</style>
