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
            v-model="stringToSearch"
          />
          <button 
          class="btn btn-outline-success" 
          type="submit"
          @click="SendString()"
          >Search</button>
        </div>
      </div>
    </nav>
  </header>
</template>

<script>
export default {
  name: "MainHeader",
  data: function () {
    return {
      stringToSearch: "",
    };
  },
  props: {},
  methods:{
    SendString(){
      this.$emit("receiveString", this.stringToSearch);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
@import "@/assets/scss/partials/_variables";
</style>
