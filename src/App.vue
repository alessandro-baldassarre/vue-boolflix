<template>
  <div id="app">
    <Header @receiveArray="arrayFinded" @receiveMovies="moviesFinded" @receiveTvShow="tvShowsFinded"/>
    <Main :arraySent="arrayReceived" :moviesSent="moviesReceived" :tvShowsSent="tvShowsReceived" />
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import Main from "./components/Main.vue";

export default {
  name: "App",
  components: {
    Header,
    Main,
  },
  data: function () {
    return {
      arrayReceived: [],
      moviesReceived: [],
      tvShowsReceived: [],
    };
  },
  methods:{
    arrayFinded(arraySearched){
      this.arrayReceived = arraySearched;
      this.moviesReceived = null;
      this.tvShowsReceived = null;
    },
    moviesFinded(arraySearched){
      this.moviesReceived = arraySearched;
      this.arrayReceived = null;
      this.tvShowsReceived = null;
      console.log(this.moviesReceived);
    },
    tvShowsFinded(arraySearched){
      this.tvShowsReceived = arraySearched;
      this.moviesReceived = null;
      this.arrayReceived = null;
    }
  }
};
</script>

<style lang="scss">
@import "@/assets/scss/style.scss";
</style>
