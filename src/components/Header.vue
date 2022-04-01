<template>
  <header>
    <nav class="navbar navbar-dark bg-dark">
      <div class="container-fluid">
        <div class="logo-brand">
          <a class="navbar-brand">Navbar</a>
          <p>{{queryString}}</p>
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
          >Search</button>
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
  methods:{
    SearchString(){
      this.queryString = this.queryString.trim().replace(/\s/g, '+');
      console.log(this.queryString);
      axios
        .get(
          `https://api.themoviedb.org/3/search/movie?api_key=d0016a6e9aa708d1a4236864958a9da3&query=${this.queryString}`
        )
        .then((response) => {
          this.findedArray = response.data.results;
          console.log(this.findedArray); 
          this.$emit("receiveArray", this.findedArray);
          this.queryString = "";
        })
        .catch((e) => {
          console.error(e);
        });
   
    }  
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
@import "@/assets/scss/partials/_variables";
</style>
