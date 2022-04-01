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
          {{flag()}}
        </li>
        <li>
          {{findedElement.popularity}}
        </li>
      </ul>
  </div>
</template>

<script>
// import { hasFlag } from 'country-flag-icons'
import getUnicodeFlagIcon from 'country-flag-icons/unicode'

export default {
  name: "MainHeader",
  props: {
    findedElement : Object,
  },
  data: function () {
    return {
      flagToSearch: "",
    };
  },
  methods:{
    flag(){
      this.flagToSearch = this.findedElement.original_language;
      if(this.flagToSearch === 'en'){
        this.flagToSearch = 'us';
      }

      return getUnicodeFlagIcon(this.flagToSearch.toUpperCase());
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
@import "@/assets/scss/partials/_variables";
</style>
