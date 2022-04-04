<template>
  <div class="card-wrapper" @mouseover="hover = true" @mouseleave="hover = false" >
    <div v-if="(hover)" class="card-info text-white">
       <ul>
      <li>
        {{ title() }}
      </li>
      <li>
        {{ originalTitle() }}
      </li>
      <li>
        {{ flag() }}
      </li>
      <li>
        {{ finalVote() }}
        <span v-for="index in finalVote()" :key="index" class="text-warning">
          <font-awesome-icon icon="fa-solid fa-star" />
        </span>
      </li>
    </ul>
    </div>
    <div v-else class="card-poster">
      <img :src="posterUrl()" :alt="'poster of ' + title()" class="rounded" />
    </div>
  </div>
</template>

<script>
// import { hasFlag } from 'country-flag-icons'
import getUnicodeFlagIcon from "country-flag-icons/unicode";

export default {
  name: "MainContentCard",
  props: {
    findedElement: Object,
  },
  data: function () {
    return {
      flagToSearch: this.findedElement.original_language,
      hover: false,
    };
  },
  methods: {
    flag() {
      if (this.flagToSearch === "en") {
        this.flagToSearch = "us";
      }

      // if(!hasFlag(this.flagToSearch.toUpperCase())){
      //   return "false"
      // }

      return getUnicodeFlagIcon(this.flagToSearch.toUpperCase());
    },
    title() {
      if (this.findedElement.title === undefined) {
        return this.findedElement.name;
      }
      return this.findedElement.title;
    },
    originalTitle() {
      if (this.findedElement.original_title === undefined) {
        return this.findedElement.original_name;
      }
      return this.findedElement.original_title;
    },
    posterUrl() {
      if (this.findedElement.poster_path !== null) {
        return (
          "https://image.tmdb.org/t/p/w342/" + this.findedElement.poster_path
        );
      }

      return "#";
    },
    finalVote() {
      return Math.round((this.findedElement.vote_average / 10) * 5);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
@import "@/assets/scss/partials/_variables";

.card-wrapper {
  height: 100%;
  cursor: pointer;
  .card-poster {
    img {
      width: 100%;
      height: 100%;
    }
  }
  .card-info{
    background-color: $mainBgDark;
    border: 1px solid black;
    border-radius: 15px;
    // transform: scale(1.2);
    height: 100%;
  }
}
</style>
