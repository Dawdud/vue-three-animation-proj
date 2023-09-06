<template>
  <div class="home">
    <Banner />
  </div>
</template>
<script lang="ts">
import { Options, Vue } from "vue-class-component";
import Banner from "@/components/Banner.vue";
@Options({
  components: {
    Banner,
  },
})
export default class Navigation extends Vue {}
</script>
