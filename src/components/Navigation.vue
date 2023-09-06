<template>
  <div class="nav-wrapper">
    <Logo />
    <nav>
      <ul>
        <li><router-link to="/">Home</router-link></li>
        <li><router-link to="/About"> About</router-link></li>
        <li><router-link to="/store">Store</router-link></li>
        <li><router-link to="/blog">Blog</router-link></li>
        <li><router-link to="/pages">All Pages</router-link></li>
      </ul>
    </nav>
  </div>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import Logo from "@/components/Logo.vue";
@Options({
  components: {
    Logo,
  },
  methods: {
    writeHello() {
      console.log("Hello it's totaly valid");
    },
  },
})
export default class Navigation extends Vue {}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.nav-wrapper {
  display: flex;
  height: 5vh;
  justify-content: center;
  align-items: center;
  padding: 1em 2em;
  box-shadow: 0 1px 8px #dddd;

  li {
    list-style: none;
    display: inline-block;
    padding: 0 1em;
  }
  a {
    text-decoration: none;
    color: #555;
    transition: all 0.3s ease 0s;
    &:hover {
      color: #b2dfdb;
    }
  }
}
</style>
