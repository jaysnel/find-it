<template>
  <div class="home">
    <h1>Find-It</h1>
    <input type="text" v-model="imgSearch" placeholder="Search Image...">
    <button @click="findImage">Search</button>
    
    <div class="img-body-content">
      <div v-for="item in imgHits" :key="item.id" class="individual-image">
        <div>
          <img :src="item.largeImageURL" alt="">
          <p>Views: {{ item.views }}</p>
          <p>Likes: {{ item.likes }}</p>
          <p>Total Downloads: {{ item.downloads }}</p>
          <a :href="item.previewURL" target="_blank">Download</a>
        </div>
      </div>
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
      imgSearch: null, //user search
      imgHits: null, //array of image search results
      hitsPerPage: 50
    }
  },
  methods: {
    findImage() {
      let page = this;
      let url = `${page.urlBase}?key=${page.apiKey}&q=${encodeURIComponent(page.imgSearch)}&image_type=photo&pretty=true&per_page=${page.hitsPerPage}`;
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

<style scoped>
.individual-image img {
    max-width: 100%;
    width: 300px;
  }


@media(min-width: 768px) {
  .img-body-content {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-gap: 5px;
    width: 85%;
    margin: auto;
  }

  .individual-image {
    max-width: 100%;
  }
}
</style>