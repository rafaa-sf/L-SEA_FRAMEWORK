<script setup>
import { onMounted, onUnmounted, ref } from 'vue';
import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';

const canvasRef = ref(null);
let renderer, scene, camera, controls, animationId;

onMounted(() => {
  const canvas = canvasRef.value;
  if (!canvas) return;

  scene = new THREE.Scene();
  scene.background = new THREE.Color(0x1a1a2e);

  camera = new THREE.PerspectiveCamera(45, window.innerWidth / window.innerHeight, 1, 1000000);
  camera.position.set(0, 50000, 100000);

  renderer = new THREE.WebGLRenderer({ canvas, antialias: true });
  renderer.setSize(window.innerWidth, window.innerHeight);
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));

  controls = new OrbitControls(camera, renderer.domElement);
  controls.enableDamping = true;
  controls.dampingFactor = 0.05;

  const ambient = new THREE.AmbientLight(0x404060, 0.5);
  scene.add(ambient);

  const dirLight = new THREE.DirectionalLight(0xffffff, 1);
  dirLight.position.set(100000, 100000, 50000);
  scene.add(dirLight);

  const animate = () => {
    animationId = requestAnimationFrame(animate);
    controls.update();
    renderer.render(scene, camera);
  };
  animate();

  const onResize = () => {
    camera.aspect = window.innerWidth / window.innerHeight;
    camera.updateProjectionMatrix();
    renderer.setSize(window.innerWidth, window.innerHeight);
  };
  window.addEventListener('resize', onResize);

  onUnmounted(() => {
    window.removeEventListener('resize', onResize);
    cancelAnimationFrame(animationId);
    controls.dispose();
    renderer.dispose();
  });
});
</script>

<template>
  <canvas ref="canvasRef" class="w-full h-full" />
</template>

<style scoped>
</style>