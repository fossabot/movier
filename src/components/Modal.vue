<template>
    <div class="outerWrapper">
        <div class="innerWrapper" v-if="!loading">
            <div class="photo">
                <img :src="data.Poster" alt="Lol">
            </div>
            <div class="description">
                <h2 class="title">{{ data.Title }}</h2>
                <span class="details">{{ data.Country }} {{ data.Year }} {{ data.Runtime }}</span>
                <p class="description">{{ data.Plot }}</p>
                <p class="genre">{{ data.Genre }}</p>
                <p class="director">{{ data.Director }}</p>
            </div>
        </div>
        <div class="close" @click="$emit('closeModal')"></div>
    </div>
</template>

<script>
import axios from 'axios';

const API_KEY = process.env.API_KEY;
const API = `https://www.omdbapi.com/?apikey=${API_KEY}`;

export default {
  name: 'Modal',
  props: {
    ID: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      itemID: null,
      loading: false,
      data: null,
    };
  },
  mounted() {
    this.itemID = this.ID;
    this.loading = true;
    axios.get(`${API}&i=${this.itemID}&plot=full`)
      .then((res) => {
        this.data = res.data;
        this.loading = false;
      })
      .catch((error) => {
        throw error;
      });
  },
};
</script>

<style lang="scss" scoped>
.outerWrapper {
  background: #fff;
  color: #000;
  max-width: 100%;
  height: 100%;
  position: fixed;
  top: 0;
  left: 0;

  @media (min-width: 1024px) {
    max-width: 70%;
    height: 60%;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    margin: auto;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.3);
  }
}
.close {
  position: absolute;
  width: 30px;
  height: 30px;
  padding: 30px;
  right: 0;
  top: 0;
  cursor: pointer;

  &::before,
  &::after {
    position: absolute;
    top: 20px;
    right: 20px;
    content: "";
    width: 20px;
    height: 2px;
    background: #000;
    display: block;
  }
  &::before {
    transform: rotate(45deg);
  }
  &::after {
    transform: rotate(-45deg);
  }
}
.innerWrapper {
  display: flex;
  height: 100%;
  padding: 50px;
  justify-content: center;
  align-items: center;
  flex-direction: column;

  @media (min-width: 1024px) {
    flex-direction: row;
    .photo{
        margin-right: 20px;
    }
  }
  .photo {
    background: #000;
    img {
      width: 250px;
      height: 400px;
    }
  }
  .title{
    margin-bottom: 0;
  }
  .description {
    color: #000;
  }
}
</style>
