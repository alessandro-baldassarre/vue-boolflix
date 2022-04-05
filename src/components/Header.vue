<template>
  <header class="sticky-top">
    <nav class="navbar navbar-dark">
      <div class="container-fluid px-5 py-2">
        <div class="logo-brand">
          <a class="navbar-brand">
            <img src="../assets/img/logo.png" alt="" />
          </a>
        </div>
        <div class="d-flex">
          <input
            class="form-control me-2"
            type="search"
            placeholder="Search"
            aria-label="Search"
            v-model="queryString"
            @keyup="SearchString()"
          />
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
  .logo-brand {
    img {
      height: 25px;
      width: 145px;
    }
  }
}
</style>
