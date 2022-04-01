<template>
  <header>
    <nav class="navbar navbar-dark bg-dark">
      <div class="container-fluid">
        <div class="logo-brand">
          <a class="navbar-brand">Navbar</a>
        </div>
        <div class="d-flex">
          <input
            class="form-control me-2"
            type="search"
            placeholder="Search"
            aria-label="Search"
            v-model="queryString"
          />
          <button
            class="btn btn-outline-success"
            type="submit"
            @click="SearchString()"
          >
            Search
          </button>
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
    };
  },
  props: {},
  methods: {
    SearchString() {
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

        this.queryString = "";
      });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
@import "@/assets/scss/partials/_variables";
</style>
