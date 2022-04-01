<template>
  <main>
    <div class="cards text-white" v-for="(film, index) in filmsList" :key="index">
      <Card />
      <ul>
        <li>
          {{film.title}}
        </li>
        <li>
          {{film.original_title}}
        </li>
        <li>
          {{film.original_language}}
        </li>
        <li>
          {{film.popularity}}
        </li>
        <li>
          {{stringSent}}
        </li>
      </ul>
    </div>
    
  </main>
</template>

<script>
import Card from "./Card.vue";
import axios from "axios";

export default {
  name: "MainHeader",
  components: {
    Card,
  },
  props: {
    stringSent: String,
  },
  data: function () {
    return {
      filmsList: null,
    };
  },
  methods: {
    getFilmsApi() {
      axios
        .get(
          `https://api.themoviedb.org/3/search/movie?api_key=d0016a6e9aa708d1a4236864958a9da3&query=ritorno+al+futuro`
        )
        .then((response) => {
          this.filmsList = response.data.results;
          console.log(this.filmsList);
        })
        .catch((e) => {
          this.errors.push(e);
        });
    },
  },
  created() {
    this.getFilmsApi();
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
@import "@/assets/scss/partials/_variables";
</style>
