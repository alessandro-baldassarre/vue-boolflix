<template>
  <main>
    <!-- div con i dati ricercati nel form -->
    <div v-if="arraySent != null" class="container-fluid p-5">
      <div class="row">
        <div class="col-12">
          <div class="select-bar px-5">
            <select
              v-model="option"
              name="select-album"
              id="select-album"
              class="bg-dark text-white"
            >
              <option class="bg-dark" value="" selected>Scegli Genere</option>
              <option
                class="bg-dark"
                v-for="(element, index) in genresAll"
                :key="index"
                :value="element"
              >
                {{ element.name }}
              </option>
            </select>
          </div>
        </div>
      </div>
      <div
        v-if="option != ''"
        class="
          row row-cols-2 row-cols-sm-3 row-cols-md-4 row-cols-lg-5 row-cols-xl-6
          p-2 p-lg-5
          gx-3
          gy-4
        "
      >
        <div
          class="col"
          v-for="(element, index) in searchedFiltered"
          :key="index"
        >
          <Card :findedElement="element" />
        </div>
      </div>
      <div
        v-else
        class="
          row row-cols-2 row-cols-sm-3 row-cols-md-4 row-cols-lg-5 row-cols-xl-6
          p-2 p-lg-5
          gx-3
          gy-4
        "
      >
        <div class="col" v-for="(element, index) in arraySent" :key="index">
          <Card :findedElement="element" />
        </div>
      </div>
    </div>

    <!-- div con i dati dei film più popolari -->
    <div
      v-else-if="arraySent == null && tvShowsSent == null && moviesSent != null"
      class="container-fluid p-5"
    >
      <div class="row">
        <div class="col-12">
          <div class="select-bar px-5">
            <select
              v-model="option"
              name="select-album"
              id="select-album"
              class="bg-dark text-white"
            >
              <option class="bg-dark" value="" selected>Scegli Genere</option>
              <option
                class="bg-dark"
                v-for="(element, index) in genresMoviesList"
                :key="index"
                :value="element"
              >
                {{ element.name }}
              </option>
            </select>
          </div>
        </div>
      </div>
      <div
        v-if="option != ''"
        class="
          row row-cols-2 row-cols-sm-3 row-cols-md-4 row-cols-lg-5 row-cols-xl-6
          p-2 p-lg-5
          gx-3
          gy-4
        "
      >
        <div
          class="col"
          v-for="(element, index) in moviesFiltered"
          :key="index"
        >
          <Card :findedElement="element" />
        </div>
      </div>
      <div
        v-else
        class="
          row row-cols-2 row-cols-sm-3 row-cols-md-4 row-cols-lg-5 row-cols-xl-6
          p-2 p-lg-5
          gx-3
          gy-4
        "
      >
        <div class="col" v-for="(element, index) in moviesSent" :key="index">
          <Card :findedElement="element" />
        </div>
      </div>
    </div>

    <!-- div con i dati delle serie TV più popolari -->
    <div
      v-else-if="arraySent == null && moviesSent == null && tvShowsSent != null"
      class="container-fluid p-5"
    >
      <div class="row">
        <div class="col-12">
          <div class="select-bar px-5">
            <select
              v-model="option"
              name="select-album"
              id="select-album"
              class="bg-dark text-white"
            >
              <option class="bg-dark" value="" selected>Scegli Genere</option>
              <option
                class="bg-dark"
                v-for="(element, index) in genresTvShowsList"
                :key="index"
                :value="element"
              >
                {{ element.name }}
              </option>
            </select>
          </div>
        </div>
      </div>
      <div
        v-if="option != ''"
        class="
          row row-cols-2 row-cols-sm-3 row-cols-md-4 row-cols-lg-5 row-cols-xl-6
          p-2 p-lg-5
          gx-3
          gy-4
        "
      >
        <div
          class="col"
          v-for="(element, index) in tvShowsFiltered"
          :key="index"
        >
          <Card :findedElement="element" />
        </div>
      </div>
      <div
        v-else
        class="
          row row-cols-2 row-cols-sm-3 row-cols-md-4 row-cols-lg-5 row-cols-xl-6
          p-2 p-lg-5
          gx-3
          gy-4
        "
      >
        <div class="col" v-for="(element, index) in tvShowsSent" :key="index">
          <Card :findedElement="element" />
        </div>
      </div>
    </div>

    <!-- div con i dati dei film e serie TV in trend -->
    <div v-else class="container-fluid p-5">
      <div class="row px-2">

        <!-- i titoli del giorno -->
        <div class="col-12">
          <h4 class="text-white">I titoli del giorno</h4>
        </div>
      </div>
      <div
        class="
          row row-cols-2 row-cols-sm-3 row-cols-md-4 row-cols-lg-5 row-cols-xl-6
          px-3
          gx-3
          flex-nowrap
          position-relative
          justify-content-center
          mb-5
        "
      >
        <!-- div che viene visualizzato in base allo scorrimento delle card -->
        <div
          class="col"
          v-for="(element, index) in responseApi"
          :key="index"
          :class="[activeElementsDay.includes(index) ? 'd-none' : '']"
        >
          <Card :findedElement="element" />
        </div>
        <div
          class="
            col
            my-next
            position-absolute
            text-white
            d-flex
            align-items-center
            justify-content-center
          "
        >
          <!-- span con l'icona per lo scorrimento della prossima card -->
          <span
            class="my-next fs-1"
            @click="nextSlide(responseApi, activeElementsDay)"
          >
            <font-awesome-icon icon="fa-solid fa-chevron-right" size="xs" />
          </span>
        </div>
        <div
          class="
            col
            my-previous
            position-absolute
            text-white
            d-flex
            align-items-center
            justify-content-center
          "
        >
          <!-- span con l'icona per lo scorrimento della card precedente -->
          <span
            class="my-previous fs-1"
            @click="prevSlide(responseApi, activeElementsDay)"
          >
            <font-awesome-icon icon="fa-solid fa-chevron-left" size="xs" />
          </span>
        </div>
      </div>
      <div class="row px-2">

        <!-- i titoli della settimana -->
        <div class="col-12">
          <h4 class="text-white">i titoli della settimana</h4>
        </div>
      </div>
      <div
        class="
          row row-cols-2 row-cols-sm-3 row-cols-md-4 row-cols-lg-5 row-cols-xl-6
          px-3
          gx-3
          flex-nowrap
          position-relative
          justify-content-center
        "
      > 
        <!-- div che viene visualizzato in base allo scorrimento delle card -->
        <div
          class="col"
          v-for="(element, index) in trendingWeek"
          :key="index"
          :class="[activeElementsWeek.includes(index) ? 'd-none' : '']"
        >
          <Card :findedElement="element" />
        </div>
        <div
          class="
            col
            my-next
            position-absolute
            text-white
            d-flex
            align-items-center
            justify-content-center
          "
        >
          <!-- span con l'icona per lo scorrimento della prossima card -->
          <span
            class="my-next fs-1"
            @click="nextSlide(trendingWeek, activeElementsWeek)"
          >
            <font-awesome-icon icon="fa-solid fa-chevron-right" size="xs" />
          </span>
        </div>
        <div
          class="
            col
            my-previous
            position-absolute
            text-white
            d-flex
            align-items-center
            justify-content-center
          "
        >

           <!-- span con l'icona per lo scorrimento della card precedente -->
          <span
            class="my-previous fs-1"
            @click="prevSlide(trendingWeek, activeElementsWeek)"
          >
            <font-awesome-icon icon="fa-solid fa-chevron-left" size="xs" />
          </span>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
import Card from "./Card.vue";
import axios from "axios";

export default {
  name: "MainContent",
  components: {
    Card,
  },
  props: {
    arraySent: Array,
    moviesSent: Array,
    tvShowsSent: Array,
  },
  data: function () {
    return {
      responseApi: null,
      trendingWeek: null,
      activeElementsDay: [],
      activeElementsWeek: [],
      nextElement: null,
      prevElement: null,
      genresMoviesList: null,
      genresTvShowsList: null,
      genresAll: null,
      option: "",
    };
  },
  methods: {
    // method per cercare attraverso l'API i trend del giorno
    callApi() {
      axios
        .get(
          `https://api.themoviedb.org/3/trending/all/day?api_key=e99307154c6dfb0b4750f6603256716d`
        )
        .then((response) => {
          this.responseApi = response.data.results;
        })
        .catch((e) => {
          console.log(e);
        });
    },
    // method per cercare attraverso l'API i trend della settimana
    callTrendWeek() {
      axios
        .get(
          `https://api.themoviedb.org/3/trending/all/week?api_key=e99307154c6dfb0b4750f6603256716d`
        )
        .then((response) => {
          this.trendingWeek = response.data.results;
        })
        .catch((e) => {
          console.log(e);
        });
    },
    // method per cercare attraverso l'API tutti i generi dei film
    callGenresMovies() {
      axios
        .get(
          `https://api.themoviedb.org/3/genre/movie/list?api_key=e99307154c6dfb0b4750f6603256716d&language=en-US`
        )
        .then((response) => {
          this.genresMoviesList = response.data.genres;
        })
        .catch((e) => {
          console.log(e);
        });
    },
    // method per cercare attraverso l'API tutti i generi delle serie TV
    callGenresTvShows() {
      axios
        .get(
          `https://api.themoviedb.org/3/genre/tv/list?api_key=e99307154c6dfb0b4750f6603256716d&language=en-US`
        )
        .then((response) => {
          this.genresTvShowsList = response.data.genres;
        })
        .catch((e) => {
          console.log(e);
        });
    },
    // method per cercare attraverso l'API tutti i generi sia dei film che delle serie tv
    callGenresAll() {
      let URL1 = `https://api.themoviedb.org/3/genre/tv/list?api_key=e99307154c6dfb0b4750f6603256716d&language=en-US`;
      let URL2 = `https://api.themoviedb.org/3/genre/movie/list?api_key=e99307154c6dfb0b4750f6603256716d&language=en-US`;
      axios
        .all([axios.get(URL1), axios.get(URL2)])
        .then((values) => {
          this.genresAll = [...values[0].data.genres, ...values[1].data.genres];
        })
        .catch((e) => {
          console.log(e);
        });
    },
    // method per l'avanzamento a destra delle card nel carosello
    nextSlide(startingArray, referenceArray) {
      console.log(referenceArray);
      if (referenceArray.length == 0) {
        this.nextElement = startingArray.length - 1;
      }
      if (this.nextElement == 5) {
        referenceArray = [];
        this.activeElementsDay = [];
        this.activeElementsWeek = [];
        this.nextElement = null;
        this.prevElement = null;
      }
      if (this.nextElement == null && referenceArray.length != 0) {
        referenceArray.pop();
      }

      if (this.nextElement != null) {
        referenceArray.push(this.nextElement);
        this.nextElement--;
      }
    },
    // method per l'avanzamento a sinistra delle card nel carosello
    prevSlide(startingArray, referenceArray) {
      console.log(referenceArray);
      if (referenceArray.length == 0) {
        this.prevElement = 0;
      }
      if (this.prevElement == 12) {
        referenceArray = [];
        this.activeElementsDay = [];
        this.activeElementsWeek = [];
        this.nextElement = null;
        this.prevElement = null;
      }
      if (this.prevElement == null && referenceArray.length != 0) {
        referenceArray.pop();
      }
      if (this.prevElement != null) {
        referenceArray.push(this.prevElement);
        this.prevElement++;
      }
    },
  },

  computed: {
    // computed per filtrare i film in base al genere scelto dall'utente
    moviesFiltered() {
      let filteredMovies = this.moviesSent.filter((movie) => {
        return movie.genre_ids.includes(this.option.id);
      });

      return filteredMovies;
    },
    // computed per filtrare le serie TV in base al genere scelto dall'utente
    tvShowsFiltered() {
      let filteredTvShows = this.tvShowsSent.filter((tvShow) => {
        return tvShow.genre_ids.includes(this.option.id);
      });

      return filteredTvShows;
    },
    // computed per filtrare la ricerca fatta dall'utente nel form in base al genere scelto dall'utente
    searchedFiltered() {
      let filteredTvShows = this.arraySent.filter((element) => {
        return element.genre_ids.includes(this.option.id);
      });

      return filteredTvShows;
    },
    genresSearchedFiltered() {
      let filteredGenres = this.gen.filter((movie) => {
        return movie.genre_ids.includes(this.option.id);
      });

      return filteredGenres;
    },
  },

  created() {
    this.callApi();
    this.callTrendWeek();
    this.callGenresMovies();
    this.callGenresTvShows();
    this.callGenresAll();
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
@import "@/assets/scss/partials/_variables";
main {
  div.my-next {
    height: 100%;
    right: -7.8%;
    background-color: rgba(0, 0, 0, 0.85);
    border-radius: 12px;
    @media screen and (max-width: 1199px) {
      right: -9.4%;
    }
    @media screen and (max-width: 991px) and (min-width: 768px) {
      right: -11.9%;
    }
    @media screen and (max-width: 767px) and (min-width: 576px) {
      right: -16%;
    }
    @media screen and (max-width: 575px) {
      right: -25%;
    }
    span.my-next {
      cursor: pointer;
    }
  }
  div.my-previous {
    height: 100%;
    left: -7.8%;
    background-color: rgba(0, 0, 0, 0.85);
    border-radius: 12px;
    @media screen and (max-width: 1199px) {
      left: -9.4%;
    }
    @media screen and (max-width: 991px) and (min-width: 768px) {
      left: -11.9%;
    }
    @media screen and (max-width: 767px) and (min-width: 576px) {
      left: -16%;
    }
    @media screen and (max-width: 575px) {
      left: -25%;
    }

    span.my-previous {
      cursor: pointer;
    }
  }
}
</style>
