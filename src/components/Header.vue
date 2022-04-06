<template>
  <header class="sticky-top">
    <nav class="navbar navbar-dark">
      <div class="container-fluid">
        <span>
          <a class="navbar-brand" href="#"
          ><img src="../assets/img/logo.png" alt=""
        /></a>
        <a class="link-light text-decoration-none me-3" href="#" @click="reloadPage()">
          Home
        </a>
        <a class="link-light text-decoration-none me-3" href="#" @click="getPopularMovies()">
          Movies
        </a>
        <a class="link-light text-decoration-none" href="#" @click="getPopularTvShows()">
          TV Shows
        </a>
        </span>
        
        <form class="d-flex justify-content-end px-4 align-items-center">
          <span
            class="icon-search text-white position-relative"
            @click="expandInput = !expandInput"
          >
            <font-awesome-icon icon="fa-solid fa-magnifying-glass" />
          </span>

          <input
            v-if="expandInput"
            class="form-control me-2 bg-dark text-white"
            type="search"
            placeholder="Search"
            aria-label="Search"
            v-model="queryString"
            @keyup="searchString()"
          />
        </form>
      </div>
    </nav>
  </header>
</template>

<script>
import axios from "axios";
export default {
  name: "MainHeader",
  data: function () {
    return {
      queryString: "",
      findedArray: null,
      expandInput: false,
      popularMovies: null,
      popularTvShows: null,
      genresList: null,
    };
  },
  props: {},
  methods: {
    searchString() {
      if (this.queryString != "") {
        let transformQuery = this.queryString.trim().replace(/\s/g, "+");

        let URL1 = `https://api.themoviedb.org/3/search/movie?api_key=d0016a6e9aa708d1a4236864958a9da3&query=${transformQuery}`;
        let URL2 = `https://api.themoviedb.org/3/search/tv?api_key=d0016a6e9aa708d1a4236864958a9da3&query=${transformQuery}`;

        axios.all([axios.get(URL1), axios.get(URL2)]).then((values) => {
          this.findedArray = [
            ...values[0].data.results,
            ...values[1].data.results,
          ];
          if (
            values[0].data.results.length === 0 &&
            values[1].data.results.length === 0
          ) {
            console.error("nessun risultato trovato");
          } else {
            this.$emit("receiveArray", this.findedArray);
          }
        });
      } else {
        this.findedArray = null;
        this.$emit("receiveArray", this.findedArray);
      }
    },
    searchGenres() {
      axios
        .get(
          `https://api.themoviedb.org/3/genre/movie/list?api_key=e99307154c6dfb0b4750f6603256716d&language=en-US`
        )
        .then((response) => {
          this.genresList = response.data.genres;
          console.log(this.genresList);
        })
        .catch((e) => {
          console.log(e);
        });
    },
    getPopularMovies() {
      axios
        .get(
          `https://api.themoviedb.org/3/movie/popular?api_key=e99307154c6dfb0b4750f6603256716d&language=en-US&page=1`
        )
        .then((response) => {
          this.popularMovies = response.data.results;
          this.$emit("receiveMovies", this.popularMovies);
        })
        .catch((e) => {
          console.log(e);
        });
    },
    getPopularTvShows() {
      axios
        .get(
          `https://api.themoviedb.org/3/tv/popular?api_key=e99307154c6dfb0b4750f6603256716d&language=en-US&page=1`
        )
        .then((response) => {
          this.popularTvShows = response.data.results;
          this.$emit("receiveTvShow", this.popularTvShows);
        })
        .catch((e) => {
          console.log(e);
        });
    },
    reloadPage() {
      window.location.reload();
    },
  },

  created() {
    this.findedArray = null;
    this.$emit("receiveArray", this.findedArray);
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
@import "@/assets/scss/partials/_variables";
header {
  background-color: $mainBgDark;

  img {
    height: 25px;
    width: 145px;
  }
  form {
    width: 30%;
  }
  input {
    padding-left: 30px;
  }
  .icon-search {
    left: 22px;
  }
}
</style>
