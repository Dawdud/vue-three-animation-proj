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
  SpotLight,
  HemisphereLight,
  SphereGeometry,
} from "three";
import { OrbitControls } from "three/addons/controls/OrbitControls.js";
const webGl = ref(null);
const scene = new Scene();
const { width, height } = useWindowSize();
const cameraTarget = { x: 0, y: 0, z: 0 };
const cameraOffset = { x: 10, y: 10, z: 10 };
const cameraSpeed = 0.00005;
const spotLightSpeed = 0.0003;
const sphereBounceSpeed = 0.00003;
const ratio = computed(() => {
  return width.value / height.value;
});
const spheres: Mesh[] = [];
let renderer: WebGLRenderer;
const countSpheres = 200;
const hemisphereLight: HemisphereLight = new HemisphereLight(
  0xff23ff,
  0x0000,
  3
);

const spotLight = new SpotLight(0xffffff, 60);
spotLight.position.set(0, -10, 0);
spotLight.angle = Math.PI / 4;
spotLight.penumbra = 1;
spotLight.decay = 2;
spotLight.distance = 0;

scene.add(spotLight);

hemisphereLight.position.set(0, -5, 0);
const camera = new PerspectiveCamera(15, ratio.value, 0.1, 1000);
camera.position.set(5, 0, -10);

scene.add(camera);
scene.add(hemisphereLight);
//scene.add(pointLight);
scene.background = new Color("rgb(194, 154, 148)");

for (let i = 0; i < countSpheres; i++) {
  const sphereSize = Math.random() * 0.6 - 0.4;
  const geometry = new SphereGeometry(sphereSize, 40, 60, 60, 20);
  const material = new MeshStandardMaterial({
    color: Math.random() * 0xffffff,
  });
  const sphere = new Mesh(geometry, material);
  sphere.position.x = Math.random() * 5 - 2;
  sphere.position.y = Math.random() * 5 - 2;
  sphere.position.z = Math.random() * 5 - 2;

  scene.add(sphere);
  spheres.push(sphere);
}

const animate = () => {
  const timer = Date.now();

  camera.position.x =
    cameraTarget.x + cameraOffset.x * Math.sin(timer * cameraSpeed);
  camera.position.z =
    cameraTarget.z + cameraOffset.z * Math.cos(timer * cameraSpeed);
  camera.position.y = cameraTarget.y + cameraOffset.y;
  camera.lookAt(cameraTarget.x, cameraTarget.y, cameraTarget.z);

  for (let i = 0, il = spheres.length; i < il; i++) {
    const sphere = spheres[i];
    //sphere.position.x = 5 * Math.cos(timer + i * 1);
    sphere.position.y = 5 * Math.sin(timer * sphereBounceSpeed + i * 9);
  }

  spotLight.position.x = Math.cos(timer * spotLightSpeed) * 2.5;
  spotLight.position.z = Math.sin(timer * spotLightSpeed) * 2.5;

  renderer.render(scene, camera);
  requestAnimationFrame(animate);
};

const setupRenderer = () => {
  renderer = new WebGLRenderer({
    canvas: webGl.value as unknown as HTMLCanvasElement,
    antialias: true,
  });
  renderer.setSize(width.value, height.value);
  renderer.render(scene, camera);
  const constrols = new OrbitControls(camera, renderer.domElement);
  //constrols.update();
};

onMounted(() => {
  setupRenderer();
  animate();
});
</script>
