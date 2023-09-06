<template>
  <div class="banner">
    <canvas ref="webGl" class="webGl" />
  </div>
</template>
<script setup lang="ts">
import { watch, onMounted, defineComponent, ref, computed } from "vue";
import { useWindowSize } from "@vueuse/core";
import {
  Scene,
  WebGLRenderer,
  Color,
  PerspectiveCamera,
  SphereGeometry,
  MeshBasicMaterial,
  Mesh,
} from "three";

const webGl = ref(null);
const scene = new Scene();
const camera = new PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
);
camera.position.z = 5;
scene.add(camera);
scene.background = new Color("rgb(0, 0, 0)");
const setupRenderer = () => {
  const renderer = new WebGLRenderer({
    canvas: webGl.value as unknown as HTMLCanvasElement,
    antialias: true,
  });
  renderer.setSize(1900, 700);
  renderer.render(scene, camera);
};

const sphere = new Mesh(
  new SphereGeometry(0.07, 200, 200),
  new MeshBasicMaterial({ color: "skyblue" })
);
scene.add(sphere);

onMounted(() => {
  setupRenderer();
});
</script>
