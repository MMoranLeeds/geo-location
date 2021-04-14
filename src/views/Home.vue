<template>
  <div class="home">
    <div class="highlight"></div>

    <div class="content">
      <h1>{{ welcomeMessage }}</h1>

      <ul>
        <li v-for="(item, i) in itemsList" v-bind:key="i">
          {{ item.name }}
        </li>
      </ul>

      <div v-if="errorStr">
        Sorry, but the following error occurred: {{ errorStr }}
      </div>

      <div class="search-assets-container">
        <button type="button" v-on:click="locateUser()">
          <svg>
            <use href="#gps" />
          </svg>
        </button>

        <input type="text" v-model="address" placeholder="Enter a location" />
      </div>
    </div>

    <section class="search-button">
      <button v-on:click="searchProperties()" type="button">
        Search assets
      </button>
    </section>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import axios from "axios";
import { gsap } from "gsap";

@Component
export default class Home extends Vue {
  welcomeMessage = "This is the home page";
  itemsList = [
    {
      name: "demo 1",
    },
    {
      name: "demo 2",
    },
    {
      name: "demo 3",
    },
    {
      name: "demo 4",
    },
    {
      name: "demo 5",
    },
  ];
  displayContent = gsap.timeline();
  gettingLocation: any = null;
  location: any = null;
  errorStr: any = null;
  address = "";

  animate() {
    this.displayContent.play();
  }

  searchProperties() {
    alert("Search logic for address " + this.address + " goes here");
  }

  async getLocation() {
    return new Promise((resolve, reject) => {
      if (!("geolocation" in navigator)) {
        reject(new Error("Geolocation is not available."));
      }

      navigator.geolocation.getCurrentPosition(
        (pos) => {
          resolve(pos);
        },
        (err) => {
          reject(err);
        }
      );
    });
  }
  async locateUser() {
    this.gettingLocation = true;
    try {
      this.gettingLocation = false;
      this.location = await this.getLocation();
      this.convertLongLat();
    } catch (e) {
      this.gettingLocation = false;
      this.errorStr = e.message;
    }
  }

  convertLongLat() {
    axios
      .get(
        "https://api.postcodes.io/postcodes?lon=" +
          this.location.coords.longitude +
          "&lat=" +
          this.location.coords.latitude
      )
      .then((response) => (this.address = response.data.result[0].postcode));
  }

  mounted() {
    this.displayContent
      .from(".search-button", {
        delay: 1.4,
        duration: 0.5,
        opacity: 0,
        y: 25,
      })
      .from(".home .highlight", {
        duration: 0.35,
        opacity: 0,
      })
      .from(".content", {
        duration: 0.35,
        opacity: 0,
        y: -10,
      }, "-=0.35")
      .from("ul li", {
        duration: 0.5,
        opacity: 0,
        y: -25,
        stagger: 0.25,
      });
    this.animate();
  }
}
</script>
