<template>
  <div class="home">
    <div class="find-it-top-contain">
      <h1>Find-It</h1>
      <div>
        <b-form-input v-model="imgSearch" placeholder="Search Image..."></b-form-input>
        <b-button variant="success" @click="findImage">Search</b-button>
      </div>
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
    <b-pagination
      v-model="currentPage"
      v-if="userLoadedImages"
      :total-rows="totalRows"
      :per-page="1"
      @input="findImage()"
      first-text="First"
      prev-text="Prev"
      next-text="Next"
      last-text="Last"
    ></b-pagination>
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
      userLoadedImages: false,
      imgSearch: null, //user search
      imgHits: null, //array of image search results
      currentPage: 1,
      hitsPerPage: 10,
      totalRows: null
    }
  },
  methods: {
    findImage() {
      let page = this;
      let url = `${page.urlBase}?key=${page.apiKey}&q=${encodeURIComponent(page.imgSearch)}&image_type=photo&pretty=true&per_page=${page.hitsPerPage}&page=${page.currentPage}`;
      
      page.userLoadedImages = false;

      axios.get(url)
      .then((res) => {
        this.imgHits = res.data.hits;
        page.userLoadedImages = true;
        page.totalRows = Math.floor(res.data.totalHits / page.hitsPerPage);
      })
      .catch((err) => {
        page.userLoadedImages = false;
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

.find-it-top-contain div{
    display: grid;
    grid-template-columns: repeat(1,1fr);
    width: 50%;
    margin: auto;
  }

.individual-image img {
    max-width: 100%;
    max-height: 100%;
    
    height: 300px;
  }


@media(min-width: 768px) {
  .find-it-top-contain div{
    width: 25%;
  }

  .img-body-content {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-gap: 5px;
    width: 85%;
    margin: auto;
  }

  .individual-image {
    max-width: 100%;
    border: 2px solid #000;
  }
}
</style>