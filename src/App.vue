<template>
  <div id="app">
    <Header @receiveString="stringToSearch" />

    <Main :stringSent="stringReceived"/>
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
      stringReceived: "",
    };
  },
  methods:{
    stringToSearch(string){
      this.stringReceived = string;
      console.log(this.stringReceived);
    }
  }
};
</script>

<style lang="scss">
@import "@/assets/scss/style.scss";
</style>
