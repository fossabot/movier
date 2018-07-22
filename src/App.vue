<template>
  <div class="app">
    <div :class="[{ flexStart: state === 1 },'wrapper']">
      <Socials :dark="state === 1"/>
      <transition name="slide">
        <p class="logo" v-if="state === 1">MOVIER</p>
      </transition>
      <transition name="fade">
        <HeroImage v-if="state === 0"/>
      </transition>
      <Claim v-if="state === 0"/>
      <SearchInput v-model="searchValue" @input="handleInput" :dark="state === 1"/>
      <div class="resultsWrapper" v-if="!loading && state === 1 && results.Response === 'True'">
        <Item
          v-for="item in results.Search.filter(element => element.Poster !== 'N/A')"
          :key="item.imdbID"
          :item="item"
          @click.native="handleModalOpen(item)"
        />
      </div>
      <div v-if="results.Error" class="error">{{ results.Error }}</div>
      <div class="loader" v-if="loading && state === 1">
      <div></div><div></div><div></div><div></div></div>
    </div>
    <Modal v-if="modalOpen" :ID="modalItemID" @closeModal="modalOpen = false"/>
  </div>
</template>

<script>
import axios from 'axios';
import Claim from '@/components/Claim.vue';
import Socials from '@/components/Socials.vue';
import Item from '@/components/Item.vue';
import Modal from '@/components/Modal.vue';
import SearchInput from '@/components/SearchInput.vue';
import HeroImage from '@/components/HeroImage.vue';


const API_KEY = process.env.VUE_APP_API_KEY;
const API = `https://www.omdbapi.com/?apikey=${API_KEY}`;

export default {
  name: 'Search',
  components: {
    Claim,
    Socials,
    Item,
    Modal,
    SearchInput,
    HeroImage,
  },
  data() {
    return {
      modalOpen: false,
      modalItemID: null,
      loading: false,
      state: 0,
      searchValue: '',
      results: [],
    };
  },
  methods: {
    handleModalOpen(item) {
      this.modalOpen = true;
      this.modalItemID = item.imdbID;
    },
    handleInput() {
      this.loading = true;
      axios.get(`${API}&type=movie&s=${encodeURIComponent(this.searchValue)}`)
        .then((res) => {
          this.results = res.data;
          this.loading = false;
          this.state = 1;
        })
        .catch((error) => {
          throw error;
        });
    },
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css?family=Montserrat:300,400,500,600,800");

* {
  box-sizing: border-box;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
body{
  font-family: "Montserrat", sans-serif;
  color: #000;
  margin: 0;
  padding: 0;
}

.fade-enter-active, .fade-leave-active{
  transition: opacity .3s ease;
}
.fade-enter, .fade-leave-to{
  opacity: 0;
}

.slide-enter-active, .slide-leave-active{
  transition: margin-top .9s ease;
}
.slide-enter, .slide-leave-to{
  margin-top: -50px;
}
.wrapper {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin: 0;
  padding: 30px;
  width: 100%;
  min-height: 100vh;

  &.flexStart{
    justify-content: flex-start;
  }
}
.resultsWrapper{
  margin-top: 50px;
  width: 270px;
  display: flex;
  flex-wrap: wrap;

  @media (min-width: 768px) {
    width: 550px;
  }

}
.error{
  margin-top: 50px;
  text-align: center;
  color: #000;
}
.logo{
  position: absolute;
  top: -10px;
  color: #000;
  letter-spacing: 8px;
  font-size: 40px;
  font-weight: bold;
}

.loader {
  display: inline-block;
  position: relative;
  margin-top: 75px;
  width: 64px;
  height: 64px;
}
.loader div {
  box-sizing: border-box;
  display: block;
  position: absolute;
  width: 51px;
  height: 51px;
  margin: 6px;
  border: 6px solid #000;
  border-radius: 50%;
  animation: loading 1.2s cubic-bezier(0.5, 0, 0.5, 1) infinite;
  border-color: #000 transparent transparent transparent;
}
.loader div:nth-child(1) {
  animation-delay: -0.45s;
}
.loader div:nth-child(2) {
  animation-delay: -0.3s;
}
.loader div:nth-child(3) {
  animation-delay: -0.15s;
}
@keyframes loading {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

</style>
