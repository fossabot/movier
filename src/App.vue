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


const API_KEY = process.env.API_KEY;
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
          console.log(error);
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
  // display: grid;
  // grid-template-columns: 1fr 1fr;
  // grid-gap: 20px;
  @media (min-width: 768px) {
    width: 550px;
    grid-template-columns: 1fr 1fr;
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

</style>
