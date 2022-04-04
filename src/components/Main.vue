<template>
  <main>
    <div class="container-fluid px-5">
      <div class="row row-cols-2 row-cols-sm-3 row-cols-md-4 row-cols-lg-5 row-cols-xl-6 pt-5 g-3">
        <div class="col" v-for="(element, index) in arraySent" :key="index">
          <Card :findedElement="element" />
        </div>
      </div>
    </div>
  </main>
</template>

<script>
import Card from "./Card.vue";

export default {
  name: "MainContent",
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
