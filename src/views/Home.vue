<template>
  <div class="home">
    <h1>Find-It</h1>
    <input type="text" v-model="imgSearch" placeholder="Search Image...">
    <button @click="findImage">Search</button>

    <div v-for="item in imgHits" :key="item.id">
      <!-- <h1>{{ item.id }}</h1> -->
      <img :src="item.largeImageURL" alt="">
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Home',
  data() {
    return {
      urlBase: "https://pixabay.com/api/",
      apiKey: process.env.VUE_APP_API_KEY,
      imgSearch: null,
      imgHits: null //array of image search results
    }
  },
  methods: {
    findImage() {
      let page = this;
      let url = `${page.urlBase}?key=${page.apiKey}&q=${encodeURIComponent(page.imgSearch)}&image_type=photo&pretty=true&per_page=50`;
      console.log(url)

      axios.get(url)
      .then((res) => {
        this.imgHits = res.data.hits;
        console.log(res.data);
      })
      .catch((err) => {
        console.log(err);
      })
    }
  }
}
</script>
