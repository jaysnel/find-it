<template>
  <div class="home">
    <div class="find-it-top-contain">
      <h1>Find-It</h1>
      <div>
        <b-form-input v-model="imgSearch" placeholder="Search Image..." @keyup.enter="findImage"></b-form-input>
        <b-button variant="success" @click="findImage">Search</b-button>
      </div>
    </div>
    
    <div class="img-body-content">
      <div v-for="item in imgHits" :key="item.id" class="individual-image">
        <div>
          <img :src="item.largeImageURL" alt="">
          <div class="image-info">
            <p><b-icon icon="eye" aria-hidden="true"></b-icon> {{ item.views }}</p>
            <p><b-icon icon="hand-thumbs-up" aria-hidden="true"></b-icon> {{ item.likes }}</p>
            <p><b-icon icon="cloud-arrow-down-fill" aria-hidden="true"></b-icon> {{ item.downloads }}</p>
            <a :href="item.pageURL" target="_blank"><b-button variant="outline-primary" size="md">Download</b-button></a>
          </div>
        </div>
      </div>
    </div>
    <div class="pagination-section" v-if="userLoadedImages">
      <div class="section1">
        <b-pagination
      v-model="currentPage"
      :total-rows="totalRows"
      :per-page="1"
      @input="findImage()"
      first-text="First"
      prev-text="Prev"
      next-text="Next"
      last-text="Last"
    ></b-pagination>
      </div>
      <div class="section2">
        <b-form-select v-model="hitsPerPage" :options="options" @change="findImage"></b-form-select>
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
      userLoadedImages: false,
      imgSearch: null, //user search
      imgHits: null, //array of image search results
      currentPage: 1,
      totalRows: null,
      ascd: true,
      hitsPerPage: 10,
      options: [
          { value: 5, text: '5' },
          { value: 10, text: '10' },
          { value: 50, text: '50' },
          { value: 100, text: '100' }
        ]
    }
  },
  methods: {
    findImage() {
      let page = this;
      let url = `${page.urlBase}?key=${page.apiKey}&q=${encodeURIComponent(page.imgSearch)}&image_type=photo&pretty=true&per_page=${page.hitsPerPage}&page=${page.currentPage}`;
      
      page.userLoadedImages = false;

      axios.get(url)
      .then((res) => {
        window.scrollTo(0,0);
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

.individual-image {
  box-shadow:0 1px 20px rgba(0, 0, 0, 0.3);
  margin: 10px;
  background: transparent;
  margin-bottom: 10px;
}

.individual-image img {
    max-width: 100%;
    max-height: 100%;
    
    height: 300px;
    padding: 10px;
  }

  .image-info {
    display: grid;
    grid-template-columns: repeat(4,1fr);
    align-items: center;

    margin: 5px;
  }

  .image-info button {
    margin-top: -15px;
  }

  .pagination {
    justify-content: center;
  }

@media(min-width: 768px) {
  .find-it-top-contain div{
    width: 25%;
  }

  .img-body-content {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-gap: 5px;
    margin: auto;
  }

  .individual-image {
    max-width: 100%;
  }
}

.pagination-section {
  display: flex;
  justify-content: center;
}

.pagination-section div {
  margin: 0 5px;
}
</style>