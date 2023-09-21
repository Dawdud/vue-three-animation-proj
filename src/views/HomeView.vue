<template>
  <div class="smooth-wrapper" ref="main">
    <Banner />
    <div class="Box1"></div>
    <div class="Box2"></div>
  </div>
</template>
<script lang="ts">
import Banner from "@/components/Banner.vue";
import gsap from "gsap";
import { ScrollTrigger } from "gsap/all";
import { onMounted, onUnmounted, ref } from "vue";
import LocomotiveScroll from "locomotive-scroll";
export default {
  components: {
    Banner,
  },
  setup() {
    let ctx: any;
    const main = ref();
    gsap.registerPlugin(ScrollTrigger);

    const setSmoothScroll = () => {
      new LocomotiveScroll({
        el: main.value,
        smooth: true,
      });
    };

    onMounted(() => {
      // setSmoothScroll();
      ctx = gsap.context(() => {
        ScrollTrigger.create({
          trigger: ".Box1",
          start: "center center",
          end: "+=300",
        });
      }, main.value);
    });
    onUnmounted(() => {
      ctx.revert();
    });
    return { main };
  },
};
</script>
<style lang="scss">
.Box1 {
  width: 100vw;
  height: 100vh;
  background-color: blue;
}
.Box2 {
  width: 100vw;
  height: 100vh;
  background-color: purple;
}
</style>
