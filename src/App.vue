<template>
  <div id="app">
    <Header @receiveArray="arrayFinded" />
    <Main :arraySent="arrayReceived"/>
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
      arrayReceived: null,
    };
  },
  methods:{
    arrayFinded(arraySearched){
      this.arrayReceived = arraySearched;
      console.log(arraySearched);
    }
  }
};
</script>

<style lang="scss">
@import "@/assets/scss/style.scss";
</style>
