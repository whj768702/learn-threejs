<template>
  <!-- <router-view/> -->
  <div>
    <canvas id="three"></canvas>
  </div>
</template>

<script lang="ts" setup>
import { onMounted } from 'vue';
import * as THREE from 'three';

onMounted(() => {
  const canvas = document.querySelector('#three');
  const renderer = new THREE.WebGLRenderer({ canvas });
  const fov = 75;
  const aspect = 2;
  const near = 0.1;
  const far = 5;
  const camera = new THREE.PerspectiveCamera(fov, aspect, near, far);
  camera.position.z = 2;

  const scene = new THREE.Scene();
  const boxWidth = 1;
  const boxHeight = 1;
  const boxDepth = 1;
  const geometry = new THREE.BoxGeometry(boxWidth, boxHeight, boxDepth);

  const color = 0xffffff;
  const intensity = 1;
  const light = new THREE.DirectionalLight(color, intensity);
  light.position.set(-1, 2, 4);
  scene.add(light);

  // eslint-disable-next-line no-shadow
  function makeInstance(geometry: any, color: any, x:number) {
    const material = new THREE.MeshPhongMaterial({ color });
    const cube = new THREE.Mesh(geometry, material);
    scene.add(cube);
    cube.position.x = x;
    return cube;
  }

  const cubes = [
    makeInstance(geometry, 0x44aa88, 0),
    makeInstance(geometry, 0x8844aa, -2),
    makeInstance(geometry, 0xaa8844, 2),
  ];

  renderer.render(scene, camera);
  function render(time:number) {
    const second = time * 0.001;
    cubes.forEach((cube, ndx) => {
      const speed = 1 + ndx * 0.1;
      const rot = second * speed;
      // eslint-disable-next-line no-param-reassign
      cube.rotation.x = rot;
      // eslint-disable-next-line no-param-reassign
      cube.rotation.y = rot;
    });
    renderer.render(scene, camera);
    requestAnimationFrame(render);
  }
  requestAnimationFrame(render);
});
</script>

<style lang="scss" scoped>
#three {
  width: 100%;
  height: 100%;
  position: fixed;
  left: 0;
  top: 0;
}
</style>
