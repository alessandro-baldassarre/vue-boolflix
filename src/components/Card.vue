<template>
  <div>
     <ul>
        <li>
          {{findedElement.title}}
        </li>
        <li>
          {{findedElement.original_title}}
        </li>
        <li>
          {{findedElement.original_language}}
        </li>
        <li>
          {{findedElement.popularity}}
        </li>
      </ul>
  </div>
</template>

<script>
export default {
  name: "MainHeader",
  props: {
    findedElement : Object,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
@import "@/assets/scss/partials/_variables";
</style>
