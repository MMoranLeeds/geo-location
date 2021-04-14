<template>
  <div class="home">
    <h1>{{ welcomeMessage }}</h1>

    <ul class="gallery">
      <li v-for="(item, i) in photos" v-bind:key="i">
        <img v-bind:src="item.url" v-bind:alt="item.title" />

        <div class="content">
          {{item.title}}
        </div>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import axios from "axios";

@Component
export default class About extends Vue {
  welcomeMessage = "This is the about page";
  photos = [];

  getImages() {
    axios
      .get("https://jsonplaceholder.typicode.com/albums/1/photos")
      .then((response) => (this.photos = response.data));
  }

  mounted() {
    this.getImages();
  }
}
</script>

<style lang="scss">
ul.gallery {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  grid-gap: 25px;

  li {
    background-color: rgb(255, 255, 255);
    box-shadow: 0 3px 8px -2px rgba(0, 0, 0, 0.15);
    border: 1px solid rgb(217, 217, 217);
    transition: all 0.3s;

    img{
      object-fit: cover;
      object-position: 50%;
    }

    .content{
      padding: 20px;
      border-top:1px solid #ddd;
    }
  }
}
</style>