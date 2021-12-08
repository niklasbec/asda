<template>
  <div id="app">
    <h1>Amazing Gif Searcher</h1>
    <input v-model="searchTerm" type="text" />
    <button class="button" @click="getGifs()">Search</button>
    <div class="gif-container">
      <img v-for="gif in gifs" :src="gif" :key="gif.id" />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchTerm: "",
      gifs: [],
    };
  },
  methods: {
    getGifs() {
      let apiKey = "dc6zaTOxFJmzC";
      let searchEndPoint = "https://api.giphy.com/v1/gifs/search?";
      let limit = 4;
      let url = `${searchEndPoint}&api_key=${apiKey}&q=${this.searchTerm}&limit=${limit}`;
      fetch(url)
        .then((response) => {
          return response.json();
        })
        .then((json) => {
          this.buildGifs(json);
        })
        .catch((err) => console.log(err));
    },
    buildGifs(json) {
      this.gifs = json.data
        .map((gif) => gif.id)
        .map((gifId) => {
          return `https://media.giphy.com/media/${gifId}/giphy.gif`;
        });
    },
  },
};
</script>

<style lang="less">
input {
  padding: 5px;
  margin-bottom: 20px;
}
.button {
  background-color: rgb(0, 172, 0);
  color: white;
  padding: 5px 20px;
  border: none;
  display: block;
  margin: 0 auto;
}
.button:hover {
  background-color: rgb(0, 148, 0);
}
.gif-container {
  margin-top: 30px;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  align-items: center;
  img {
    width: 50%;
  }
}
</style>