<template>
  <div class="home">
    <div class="find-it-top-contain">
      <h1>Find-It</h1>
      <input type="text" v-model="imgSearch" placeholder="Search Image...">
      <button @click="findImage">Search</button>
    </div>
    
    <div class="img-body-content">
      <div v-for="item in imgHits" :key="item.id" class="individual-image">
        <div>
          <img :src="item.largeImageURL" alt="">
          <p>Views: {{ item.views }}</p>
          <p>Likes: {{ item.likes }}</p>
          <p>Total Downloads: {{ item.downloads }}</p>
          <a :href="item.pageURL" target="_blank">Download</a>
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
      pageNumber: 1,
      hitsPerPage: 50
    }
  },
  methods: {
    findImage() {
      let page = this;
      let url = `${page.urlBase}?key=${page.apiKey}&q=${encodeURIComponent(page.imgSearch)}&image_type=photo&pretty=true&per_page=${page.hitsPerPage}&page=${page.pageNumber}`;
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
.find-it-top-contain {
  background: url("../assets/images/black-camera.jpg");
  background-size: cover;
  background-position: center center;
  background-repeat: no-repeat;
  height: 25vh;
  margin: 0;
  padding: 0;
}

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