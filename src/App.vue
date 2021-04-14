<template>
  <div id="app">
    <header>
      <button type="button" v-on:click="logicAlert('Display Menu')">
        <svg>
          <use href="#menu" />
        </svg>
      </button>

      <router-link to="/">
        <svg class="logo">
          <use href="#logo" />
        </svg>
      </router-link>

      <button type="button" v-on:click="logicAlert('Display User Panel')">
        <svg>
          <use href="#user" />
        </svg>
      </button>
    </header>

    <router-view />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import { gsap } from "gsap";

@Component
export default class App extends Vue {
  displayContent = gsap.timeline();

  animate() {
    this.displayContent.play();
  }

  logicAlert(message: string) {
    alert(message);
  }

  mounted() {
    this.displayContent
      .from("header", {
        delay: 0.3,
        duration: 0.7,
        opacity: 0,
        y: -100,
      })
      .from("header > svg, header button", {
        duration: 1,
        opacity: 0,
      });
    this.animate();
  }
}
</script>

<style lang="scss">
#app {
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}

h3 {
  margin: 40px 0 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
