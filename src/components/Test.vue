<template>
  <div class="test" ref="container"></div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

import * as THREE from "three";
import { STLLoader } from "three/examples/jsm/loaders/STLLoader";
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';
import Stats from "three/examples/jsm/libs/stats.module";

export default defineComponent({
  name: "Test",
  mounted() {
    const container = this.$refs.container as HTMLDivElement;

    const scene = new THREE.Scene();
    scene.add(new THREE.AxesHelper(5));

    const light = new THREE.SpotLight();
    light.position.set(40, 0, 100);
    light.power = 1000;
    scene.add(light);

    const light1 = new THREE.SpotLight();
    light1.position.set(-20, -20, -20);
    light1.power = 1000;
    scene.add(light1);

    const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
    camera.position.set(100, 0, 100);

    const renderer = new THREE.WebGLRenderer();
    renderer.outputEncoding = THREE.sRGBEncoding;
    renderer.setSize(window.innerWidth, window.innerHeight);
    document.body.appendChild(renderer.domElement);

    const controls = new OrbitControls(camera, renderer.domElement);
    controls.enableDamping = true;

    // const envTexture = new THREE.CubeTextureLoader().load(["img/px_50.png", "img/nx_50.png", "img/py_50.png", "img/ny_50.png", "img/pz_50.png", "img/nz_50.png"]);
    // envTexture.mapping = THREE.CubeReflectionMapping;

    const material = new THREE.MeshPhysicalMaterial({
      color: 0xb2ffc8,
      // envMap: envTexture,
      metalness: 0.7,
      roughness: 0.1,
      opacity: 1.0,
      transparent: false,
      transmission: 0.99,
      clearcoat: 1.0,
      clearcoatRoughness: 0.25,
    });

    const loader = new STLLoader();
    loader.load(
      '/models/levetta_test.stl',
      function (geometry) {
        const mesh = new THREE.Mesh(geometry, material);
        scene.add(mesh);
      },
      (xhr) => {
        console.log((xhr.loaded / xhr.total) * 100 + "% loaded");
      },
      (error) => {
        console.log(error);
      }
    );

    window.addEventListener("resize", onWindowResize, false);
    function onWindowResize() {
      camera.aspect = window.innerWidth / window.innerHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(window.innerWidth, window.innerHeight);
      render();
    }

    const stats = Stats();
    container.appendChild(stats.dom);

    function animate() {
      requestAnimationFrame(animate);

      controls.update();

      render();

      stats.update();
    }

    function render() {
      renderer.render(scene, camera);
    }

    animate();
  },
});
</script>

<style scoped lang="scss">
</style>
