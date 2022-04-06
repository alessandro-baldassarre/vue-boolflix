<template>
<!-- header -->
  <header class="sticky-top">
    <!-- navbar -->
    <nav>
      <div
        class="
          container-fluid
          px-5
          py-3
          d-flex
          justify-content-between
          align-items-center
        "
      >
        <div>
          <!-- logo-brand + links -->
          <span>
            <a class="navbar-brand" href="#"
            @click="reloadPage()"
              ><img src="../assets/img/logo.png" alt=""
            /></a>
            <a
              class="link-light text-decoration-none me-3"
              href="#"
              @click="reloadPage()"
            >
              Home
            </a>
            <a
              class="link-light text-decoration-none me-3"
              href="#"
              @click="getPopularMovies()"
            >
              Movies
            </a>
            <a
              class="link-light text-decoration-none"
              href="#"
              @click="getPopularTvShows()"
            >
              TV Shows
            </a>
          </span>
        </div>

        <!-- search form + user account -->
        <div>
          <div class="d-flex align-items-center justify-content-center">
            <div class="me-3 position-relative">
              <span class="icon-search text-white position-absolute fs-5">
                <font-awesome-icon
                  icon="fa-solid fa-magnifying-glass"
                  @click="expandInput = !expandInput"
                />
              </span>
              <input
                class="form-control bg-dark text-white ps-2"
                :class="expandInput ? 'visible' : 'invisible'"
                type="search"
                placeholder="Movies, TV Shows.."
                aria-label="Search"
                v-model="queryString"
                @keyup="searchString()"
              />
            </div>

            <span class="text-white d-flex align-items-center fs-4 ">
              <font-awesome-icon icon="fa-solid fa-circle-user" />
            </span>
          </div>
        </div>
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
    };
  },
  props: {},
  methods: {
    // method per cercare attraverso l'API la stringa presa dal form di ricerca
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
   // method per cercare attraverso l'API i film più popolari
    getPopularMovies() {
      axios
        .get(
          `https://api.themoviedb.org/3/movie/popular?api_key=e99307154c6dfb0b4750f6603256716d&language=en-US&page=1`
        )
        .then((response) => {
          this.popularMovies = response.data.results;
          // mando all'app il risulato della richiesta API attraverso un emit sucessivamente da mandare al main attraverso i props per visualizzare i giusti contenuti
          this.$emit("receiveMovies", this.popularMovies);
        })
        .catch((e) => {
          console.log(e);
        });
    },
    // method per cercare attraverso l'API le serie TV più popolari
    getPopularTvShows() {
      axios
        .get(
          `https://api.themoviedb.org/3/tv/popular?api_key=e99307154c6dfb0b4750f6603256716d&language=en-US&page=1`
        )
        .then((response) => {
          this.popularTvShows = response.data.results;
          // mando all'app il risulato della richiesta API attraverso un emit sucessivamente da mandare al main attraverso i props per visualizzare i giusti contenuti
          this.$emit("receiveTvShow", this.popularTvShows);
        })
        .catch((e) => {
          console.log(e);
        });
    },
    // method per ricaricare la pagina
    reloadPage() {
      window.location.reload();
    },
  },

  created() {
    // alla creazione mando all'app un emit sucessivamente da mandare al main attraverso i props per visualizzare i giusti contenuti
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
  height: 80px;

  img {
    height: 25px;
    width: 145px;
  }
  input {
    padding-left: 30px;
  }
  .icon-search {
    right: 10px;
    top: 50%;
    transform: translate(0,-50%);

  }
}
</style>
