<template>
  <Navigation></Navigation>
  <router-view></router-view>
</template>
<script lang="ts">
import { defineComponent } from "vue";
import Navigation from "@/components/Navigation.vue"; // @ is an alias to /src

export default defineComponent({
  name: "App",
  components: {
    Navigation,
  },
});
</script>

<style lang="scss">
html,
body {
  margin: 0px;
  padding: 0px;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
