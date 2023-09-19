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
  MeshStandardMaterial,
  Mesh,
  PointLight,
  HemisphereLight,
  SphereGeometry,
} from "three";
import { OrbitControls } from "three/addons/controls/OrbitControls.js";
const webGl = ref(null);
const scene = new Scene();

const spheres: any = [];
let renderer: WebGLRenderer;
const countSpheres = 200;
const secondLight = new HemisphereLight(0xff23ff, 0x0000, 2);
const light = new PointLight(0xe6e6e6, 20, 100);

light.position.set(0, -5, 0);
const camera = new PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
);
camera.position.z = 5;

scene.add(camera);
scene.add(secondLight);

scene.background = new Color("rgb(0, 0, 0)");
scene.add(light);

for (let i = 0; i < countSpheres; i++) {
  const sphereSize = Math.random() * 0.001 - 0.5;
  const geometry = new SphereGeometry(sphereSize, 40, 60, 60, 20);
  const material = new MeshStandardMaterial({
    color: Math.random() * 0xffffff,
  });
  const sphere = new Mesh(geometry, material);
  sphere.position.x = Math.random() * 10 - 6;
  sphere.position.y = Math.random() * 10 - 5;
  sphere.position.z = Math.random() * 10 - 6;

  scene.add(sphere);
  spheres.push(sphere);
}

const animate = () => {
  const timer = 0.00003 * Date.now();

  for (let i = 0, il = spheres.length; i < il; i++) {
    const sphere = spheres[i];

    sphere.position.x = 5 * Math.cos(timer + i * 9);
    sphere.position.y = 5 * Math.sin(timer + i * 0.2);
  }

  renderer.render(scene, camera);
  requestAnimationFrame(animate);
};

const setupRenderer = () => {
  renderer = new WebGLRenderer({
    canvas: webGl.value as unknown as HTMLCanvasElement,
    antialias: true,
  });
  renderer.setSize(1920, 680);
  renderer.render(scene, camera);
  const constrols = new OrbitControls(camera, renderer.domElement);
  constrols.update();
};

onMounted(() => {
  setupRenderer();
  animate();
});
</script>
