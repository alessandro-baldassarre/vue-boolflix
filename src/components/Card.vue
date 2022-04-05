<template>
  <div
    class="card-wrapper"
    @mouseover="hover = true"
    @mouseleave="hover = false"
  >
    <div v-if="hover && !clickInfo" class="card-info text-white p-1">
      <img
        class="card-info-img"
        :src="posterUrl()"
        :alt="'poster of ' + title()"
      />
      <div
        class="
          card-info-text
          pt-4
          ps-md-1
          d-flex
          flex-column
          justify-content-evenly
          position-relative
        "
      >
        <div class="icon-find-more position-absolute" @click="clickInfo = true">
          <span
            class="
              icon-find-more-icon
              rounded-circle
              border border-light
              d-inline-block d-flex
              align-items-center
              justify-content-center
              p-1
            "
          >
            <font-awesome-icon icon="fa-solid fa-chevron-down" size="xs" />
          </span>
          <span class="icon-find-more-text position-absolute">
            Altre info
          </span>
        </div>

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
            <span
              v-for="index in finalVote()"
              :key="index"
              class="text-warning"
            >
              <font-awesome-icon icon="fa-solid fa-star" />
            </span>
          </div>
        </div>
      </div>
    </div>
    <div
      v-else-if="hover && clickInfo"
      class="overview text-white p-2 d-flex flex-column align-items-end"
    >
      <span class="p-1 mb-3 cursor-pointer" @click="clickInfo = false">
        <font-awesome-icon icon="fa-solid fa-chevron-down" />
      </span>
      Overview: <br />{{ findedElement.overview }}
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
      clickInfo: false,
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

div.card-wrapper {
  height: 100%;
  position: relative;
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
  .card-info ,
  .overview{
    background-color: #181818;
    height: 440px;
    width: 330px;
    left: -22%;
    top: -12%;
    box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px;
    border-radius: 15px;
    z-index: 2;
    position: absolute;
    overflow-y: auto;
    @media screen and (max-width: 1500px) and (min-width: 1300px) {
      height: 350px;
      width: 250px;
      left: -12%;
      top: -8%;
    }
    @media screen and (max-width: 1299px) and (min-width: 952px) {
      height: 340px;
      width: 250px;
      left: -12%;
      top: -10%;
    }
    @media screen and (max-width: 951px) and (min-width: 768px) {
      height: 330px;
      width: 230px;
      left: -12%;
      top: -12%;
    }
    @media screen and (max-width: 767px) and (min-width: 650px) {
      height: 330px;
      width: 240px;
      left: -12%;
      top: -8%;
    }
    @media screen and (max-width: 649px) and (min-width: 577px){
      height: 300px;
      width: 200px;
      left: -12%;
      top: -12%;
    }
     @media screen and (max-width: 576px) and (min-width: 481px){
      height: 370px;
      width: 240px;
      left: -12%;
      top: -12%;
    }
    @media screen and (max-width: 480px){
      height: 300px;
      width: 200px;
      left: -12%;
      top: -12%;
    }
    img {
      width: 100%;
      height: 50%;
      border-radius: 15px;
      object-fit: cover;
      object-position: top;
    }
    .card-info-text {
      height: 50%;
      font-size: 0.6rem;
      @media screen and (min-width: 1200px) {
        font-size: 1rem;
      }
      @media screen and (min-width: 992px) {
        font-size: 0.7rem;
      }
      @media screen and (min-width: 768px) {
        font-size: 0.6rem;
      }
      @media screen and (min-width: 576px) {
        font-size: 0.6rem;
      }
    }
    div.icon-find-more {
      top: 18px;
      right: 10px;
      &:hover span.icon-find-more-text {
        display: block;
        bottom: 18px;
        left: -50%;
      }
      span.icon-find-more-icon {
        cursor: pointer;
        &:hover {
          background-color: $mainBgDark;
        }
      }
      span.icon-find-more-text {
        display: none;
        width: 150px;
        font-size: 0.4rem;
      }
    }
  }

  // .overview {
  //   background-color: #181818;
  //   height: 400px;
  //   width: 280px;
  //   left: -12%;
  //   top: -12%;
  //   box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px;
  //   border-radius: 15px;
  //   font-size: 0.6rem;
  //   overflow-y: auto;
  //   z-index: 2;
  //   position: absolute;
  // }
}
</style>
