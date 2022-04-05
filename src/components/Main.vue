<template>
  <main>
    <div v-if="arraySent != null" class="container-fluid p-5">
      <div
        class="
          row row-cols-2 row-cols-sm-3 row-cols-md-4 row-cols-lg-5 row-cols-xl-6
          p-2 p-lg-5
          gx-3
          gy-4
        "
      >
        <div class="col" v-for="(element, index) in arraySent" :key="index">
          <Card :findedElement="element" />
        </div>
      </div>
    </div>
    <div v-else class="container-fluid p-5">
      <div class="row px-2">
        <div class="col-12">
          <h4 class="text-white">I titoli del momento</h4>
        </div>
      </div>
      <div
        class="
          row row-cols-2 row-cols-sm-3 row-cols-md-4 row-cols-lg-5 row-cols-xl-6
          px-3
          gx-3
          flex-nowrap
          position-relative
          justify-content-center
        "
      >
        <div
          class="col"
          v-for="(element, index) in responseApi"
          :key="index"
          :class="[activeElements.includes(index) ? 'd-none' : '']"
        >
          <Card :findedElement="element" />
        </div>
        <div
          class="
            col
            my-next
            position-absolute
            text-white
            d-flex
            align-items-center
            justify-content-center
          "
        >
          <span class="my-next fs-1" @click="nextSlide()">
            <font-awesome-icon icon="fa-solid fa-chevron-right" size="xs" />
          </span>
        </div>
        <div
          class="
            col
            my-previous
            position-absolute
            text-white
            d-flex
            align-items-center
            justify-content-center
          "
        >
          <span class="my-previous fs-1" @click="prevSlide()">
            <font-awesome-icon icon="fa-solid fa-chevron-left" size="xs" />
          </span>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
import Card from "./Card.vue";
import axios from "axios";

export default {
  name: "MainContent",
  components: {
    Card,
  },
  props: {
    arraySent: Array,
  },
  data: function () {
    return {
      responseApi: null,
      // activeElements: [0,1,2,3,4,5,6,7,16,17,18,19,20,21],
      activeElements: [],
      nextElement: null,
      prevElement: null,
    };
  },
  methods: {
    callApi() {
      axios
        .get(
          `https://api.themoviedb.org/3/trending/all/day?api_key=e99307154c6dfb0b4750f6603256716d`
        )
        .then((response) => {
          this.responseApi = response.data.results;
        })
        .catch((e) => {
          console.log(e);
        });
    },
    nextSlide() {
      if (this.activeElements.length == 0){
        this.nextElement = this.responseApi.length - 1;
      }
      if(this.nextElement == 5){
        this.activeElements = [];
        return;
      }
      if(this.nextElement == null && this.activeElements.length != 0){
        this.activeElements.pop();
        return;
      }
      this.activeElements.push(this.nextElement);
      this.nextElement --;
    },
    prevSlide() {
      if (this.activeElements.length == 0){
        this.prevElement = 0;
      }
      if(this.prevElement == 12){
        this.activeElements = [];
        return;
      }
      if(this.prevElement == null && this.activeElements.length != 0){
        this.activeElements.pop();
        return;
      }
        this.activeElements.push(this.prevElement);
        this.prevElement++;
      }
  },
  created() {
    this.callApi();
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
@import "@/assets/scss/partials/_variables";

div.my-next {
  height: 100%;
  right: -7.8%;
  background-color: rgba(0, 0, 0, 0.85);
  border-radius: 12px;
  @media screen and (max-width: 1199px) {
    right: -9.4%;
  }
  @media screen and (max-width: 991px) and (min-width: 768px) {
    right: -11.9%;
  }
  @media screen and (max-width: 767px) and (min-width: 576px) {
    right: -16%;
  }
  @media screen and (max-width: 575px) {
    right: -25%;
  }
  span.my-next {
    cursor: pointer;
  }
}
div.my-previous {
  height: 100%;
  left: -7.8%;
  background-color: rgba(0, 0, 0, 0.85);
  border-radius: 12px;
  @media screen and (max-width: 1199px) {
    left: -9.4%;
  }
  @media screen and (max-width: 991px) and (min-width: 768px) {
    left: -11.9%;
  }
  @media screen and (max-width: 767px) and (min-width: 576px) {
    left: -16%;
  }
  @media screen and (max-width: 575px) {
    left: -25%;
  }

  span.my-previous {
    cursor: pointer;
  }
}
</style>
