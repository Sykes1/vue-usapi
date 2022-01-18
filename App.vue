<template>
  <div id="app">
    <div class="container">
       <img class="img-fluid logo1" alt="logo" src="./assets/logo.png">
      <h2 class="text-center mt-3">Choose an Element</h2>
      <div class="button-wrapper">
        <button class="btn" @click="searchUnsplash('water')">water photos</button>
        <button class="btn" @click="searchUnsplash('fire')">fire photos</button>
        <button class="btn" @click="searchUnsplash('wind')">wind photos</button>
      </div>
      <stack
              :column-min-width="300"
              :gutter-width="15"
              :gutter-height="15"
              monitor-images-loaded
      >
        <stack-item
                v-for="(image, i) in images"
                :key="i"
                style="transition: transform 300ms"
        >
          <img :src="image.urls.small" :alt="image.alt_description" />
        </stack-item>
      </stack>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { Stack, StackItem } from "vue-stack-grid";

export default {
  name: "app",
  components: {
    Stack,
    StackItem
  },
  data: () => ({
    images: []
  }),
  methods: {
    searchUnsplash(topic) {
      this.images = [];
      axios
        .get(
          `https://api.unsplash.com/search/photos?query=${topic}&per_page=30`,
          {
            headers: {
              Authorization:
                "Client-ID yxv_bt_9MA0vQI42m-2LjFzGU85eLOQg39ziXc1cp1I",
              "Accept-Version": "v1"
            }
          }
        )
        .then(response => {
          this.images = response.data.results;
        })
        .catch(() => {
          this.images = [];
        });
    }
  }
};
</script>

<style>
body {
    color: #ffffff;
    background-color: #000000;
}
h2 {
  color:#ffffff!important;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 20px;
}
.container {
  width: 80vw;
  margin: 0 auto;
}
.button-wrapper {
  display: flex;
  justify-content: center;
  margin-bottom: 25px;
}
.btn {
  font-size: 18px;
  background-color: #b0d000;
  color: white;
  padding: 20px 40px;
  border-radius: 0;
  text-transform: uppercase;
  font-weight: bold;
}
.btn:hover {
    color: #ffffff;
    background: #889d1c;
}
.btn:not(:last-child) {
  margin-right: 10px;
}
img {
  width: 100%;
  height: auto;
  border-radius: 12px;
}
.logo1 {
  max-width:350px;
}
</style>
