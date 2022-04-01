<template>
  <main>
    <div class="cards text-white" v-for="(film, index) in arraySent" :key="index">
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
      </ul>
    </div>
    
  </main>
</template>

<script>
import Card from "./Card.vue";


export default {
  name: "MainHeader",
  components: {
    Card,
  },
  props: {
    arraySent: Array,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
@import "@/assets/scss/partials/_variables";
</style>
