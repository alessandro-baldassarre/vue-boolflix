<template>
  <main>
    <div class="cards text-white" v-for="(element, index) in arraySent" :key="index">
      <Card :findedElement="element"/>
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
