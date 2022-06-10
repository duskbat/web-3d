<template>
  <head>
    <meta charset="utf-8">
    <title>My first three.js app</title>
  </head>

  <div class="box">

  </div>
</template>

<script>
import * as THREE from 'three';
import {OrbitControls} from "three/examples/jsm/controls/OrbitControls";

export default {
  name: 'Web3D',

  mounted() {
    this.init();
  },

  methods: {
    init() {
      const scene = new THREE.Scene();
      const camera = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 1, 1000);

      const renderer = new THREE.WebGLRenderer();
      renderer.setSize(window.innerWidth, window.innerHeight);
      document.body.appendChild(renderer.domElement);

      let axis = new THREE.AxesHelper(10);
      scene.add(axis);

      const geometry = new THREE.BoxGeometry(1, 1, 1);
      const material = new THREE.MeshNormalMaterial();
      const cube = new THREE.Mesh(geometry, material);
      cube.position.y = 5;
      scene.add(cube);

      camera.position.z = 5;
      camera.position.x = 5;
      camera.position.y = 5;

      camera.lookAt(0, 0, 0);

      let controls = new OrbitControls(camera, renderer.domElement);
      controls.minDistance = 1;
      controls.maxDistance = 10;
      //controls.minPolarAngle = Math.PI / 3; //与水平夹角
      //controls.maxPolarAngle = Math.PI / 3;

      function animate() {
        requestAnimationFrame(animate);

        cube.rotation.x += 0.01;
        cube.rotation.y += 0.05;

        renderer.render(scene, camera);
      }

      animate();
    }

  }
}
</script>

<style>
body {
  margin: 0;
}
</style>
