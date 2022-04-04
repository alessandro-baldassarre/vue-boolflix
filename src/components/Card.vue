<template>
  <div
    class="card-wrapper"
    @mouseover="hover = true"
    @mouseleave="hover = false"
  >
    <div v-if="hover" class="card-info text-white p-1">
      <img
        class="card-info-img"
        :src="posterUrl()"
        :alt="'poster of ' + title()"
      />
      <div class="card-info-text pt-3 ps-1 d-flex flex-column justify-content-evenly">
        <div>Titolo: {{ title() }}</div>
        <div
          v-if="
            findedElement.original_title != findedElement.title ||
            findedElement.name != findedElement.original_name
          "
        >
          Titolo originale: {{ originalTitle() }}
        </div>
        <div>Lingua: {{ flag() }}</div>
        <div class="d-flex justify-content-between">
          <div class="vote me-4">
            Voto:
          <span v-for="index in finalVote()" :key="index" class="text-warning">
            <font-awesome-icon icon="fa-solid fa-star" />
          </span>
          </div>
          <div class="icon-find-more pe-3">
            <span class="rounded-circle border border-light p-1">
              <font-awesome-icon icon="fa-solid fa-chevron-down" />
            </span>
          </div>
        </div>
      </div>

      <!-- <li class="overview">
       Overview: {{ findedElement.overview}}
      </li> -->
    </div>
    <div v-else class="card-poster">
      <img
        class="card-poster-img"
        :src="posterUrl()"
        :alt="'poster of ' + title()"
      />
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

      return "https://www.translationvalley.com/wp-content/uploads/2020/03/no-iamge-placeholder.jpg";
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
  .card-poster {
    height: 100%;
    img {
      width: 100%;
      height: 100%;
      border-radius: 15px;
      object-fit: cover;
      object-position: center;
    }
  }
  .card-info {
    background-color: #181818;
    height: 100%;
    box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px;
    border-radius: 15px;
    transform: scale(1.6, 1.24);
    img {
      width: 100%;
      height: 50%;
      border-radius: 15px;
      object-fit: cover;
      object-position: top;
    }
    .card-info-text{
      height: 50%;
      font-size: .8rem;
      @media screen and (max-width: 1250px) {
        font-size: .65rem;
      }
    }
    .icon-find-more{
      span{
        cursor: pointer;
        &:hover{
          background-color: $mainBgDark;
        }
      }
    }
  }
  .overview {
    max-height: 30%;
    overflow-y: auto;
  }
}
</style>
